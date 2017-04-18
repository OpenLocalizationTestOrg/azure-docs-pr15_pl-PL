<properties
   pageTitle="Wdrażanie wielu wystąpień zasobów | Microsoft Azure"
   description="Za pomocą kopiowania i tablice w szablonie Menedżera zasobów Azure iteracyjne wielokrotnie, wdrażając zasobów."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# <a name="create-multiple-instances-of-resources-in-azure-resource-manager"></a>Tworzenie wielu wystąpień zasobów w Menedżerze zasobów Azure

W tym temacie przedstawiono sposoby przejść w szablonie Azure Menedżera zasobów, aby utworzyć wielu wystąpień zasobu.

## <a name="copy-copyindex-and-length"></a>kopiowanie, copyIndex i długości

W ramach zasobu, aby utworzyć wiele razy można zdefiniować **Kopiuj** obiekt określająca liczbę iteracji. Kopiuj ma następujący format:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Masz dostęp do bieżącej wartości iteracji funkcją **copyIndex()** , tak jak pokazano poniżej poziomu funkcja "concat".

    [concat('examplecopy-', copyIndex())]

Podczas tworzenia wielu zasobów z tablicę wartości, możesz użyć funkcji **długość** , do określenia liczby. Tablica podane jako parametr funkcji długość.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## <a name="use-index-value-in-name"></a>Użyj wartości z pola indeksu w polu Nazwa

Możesz użyć kopii operacji tworzenia wielu wystąpień zasób jednoznacznie są nazywane w oparciu o kolejnym indeksu. Na przykład można dodać unikatowy numer na końcu nazw poszczególnych zasobów, który jest używany. Aby wdrożyć trzy witryny sieci web o nazwie:

- examplecopy 0
- examplecopy-1
- examplecopy-2.

Użyj następującego szablonu:

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## <a name="offset-index-value"></a>Wartości przesunięcia indeksu

Można zauważyć w poprzednim przykładzie wartość indeksu prowadzące od 0 do 2. Aby przesunąć wartość indeksu, można przekazać wartość w funkcji **copyIndex()** , takiej jak **copyIndex(1)**. Liczba iteracji, aby wykonać nadal jest określona w elemencie Kopiuj, ale jest zwracana wartość copyIndex w określonej wartości. Tak przy użyciu tego samego szablonu, co w poprzednim przykładzie, ale określanie **copyIndex(1)** chcesz wdrożyć trzy witryny sieci web o nazwie:

- examplecopy-1
- examplecopy-2
- examplecopy-3

## <a name="use-copy-with-array"></a>Kopiowanie za pomocą tablicy
   
Operacja kopiowania jest szczególnie pomocne podczas pracy z tablicami, ponieważ można wykonać iterację każdego elementu w tablicy. Aby wdrożyć trzy witryny sieci web o nazwie:

- examplecopy Contoso
- examplecopy Fabrikam
- examplecopy Coho

Użyj następującego szablonu:

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

Oczywiście liczba kopii należy ustawić na wartość inną niż długość tablicy. Na przykład można utworzyć tablicę z wielu wartości, a następnie przekazać w wartości parametru określająca liczbę elementów tablicy do wdrożenia. W tym przypadku należy ustawić liczba kopii, jak pokazano w przykładzie pierwszy. 

## <a name="depending-on-resources-in-a-loop"></a>W zależności od zasobów w pętli

Można określić, czy zasób należy wdrożyć po innego zasobu za pomocą elementu **dependsOn** . Gdy trzeba wdrożyć zasobu, która jest zależna od zbiór zasobów w pętli, można użyć podać nazwę pętli kopii w elemencie **dependsOn** . W poniższym przykładzie pokazano, jak wdrożyć 3 kont miejsca do magazynowania przed wdrożeniem maszyny wirtualnej. Pełna definicji maszyn wirtualnych nie jest wyświetlane. Zauważyć, że element kopii ma **nazwę** grupy **storagecopy** a elemencie **dependsOn** maszyn wirtualnych również jest ustawiona na **storagecopy**.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## <a name="looping-on-a-nested-resource"></a>Powtarzanie zagnieżdżonych zasobu

Nie można użyć pętli kopii zagnieżdżonych zasobów. Jeśli chcesz utworzyć wielu wystąpień zasobu, która zwykle określają jako zagnieżdżona w inny zasób, możesz należy zamiast tego utworzyć zasób jako zasób najwyższego poziomu i definiowanie relacji z zasobem nadrzędnej przez ustawienie właściwości **Typ** i **Nazwa** .

Załóżmy, że zwykle określają zestaw danych jako zasób zagnieżdżone wewnątrz fabrycznych danych.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Aby utworzyć wielu wystąpień zestawy danych, należy zmienić swój szablon, tak jak pokazano poniżej. Powiadomienie o w pełni kwalifikowaną typ i nazwa zawiera nazwę factory danych.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## <a name="create-multiple-instances-when-copy-wont-work"></a>Tworzenie wielu wystąpień, gdy kopia nie będzie działać.

**Kopiowanie** można używać tylko na typów zasobów, a nie na właściwości w ramach typu zasobu. Może to spowodować problemy dla Ciebie, umożliwia tworzenie wielu wystąpień elementu, który jest częścią zasobu. Typowy scenariusz jest utworzenie wielu dyskach danych dla maszyny wirtualnej. Nie można użyć **kopii** dysków danych, ponieważ **dataDisks** jest właściwością na tym komputerze wirtualnych nie własny typ zasobu. Zamiast tego możesz utworzyć tablicę tyle dysków danych będzie potrzebny, a przekazać rzeczywistej liczby dysków danych do utworzenia. W definicji maszyn wirtualnych funkcja **wykonać** uzyskanie tylko liczbę elementów, które są potrzebne w tablicy.

Pełna przykładem tego wzorca jest Pokaż w oknie [Tworzenie maszyn wirtualnych z zaznaczonym dynamiczne dysków danych](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) szablonu.

Poniżej przedstawiono odpowiednich sekcjach szablonu wdrożenia. Wiele szablon został usunięty do wyróżnienia w sekcjach zajmujących się dynamicznie tworzenia liczbę dyski danych. Zwróć uwagę, parametr **numDataDisks** , który umożliwia przekazywanie liczby dysków do utworzenia. 

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```


## <a name="next-steps"></a>Następne kroki
- Jeśli chcesz uzyskać informacje o części szablonu, zobacz [Tworzenie szablonów Menedżera zasobów Azure](./resource-group-authoring-templates.md).
- Wszystkie funkcje, których można używać w szablonie zobacz [Funkcje szablonu Menedżera zasobów Azure](./resource-group-template-functions.md).
- Aby dowiedzieć się, jak wdrożyć szablonu, zobacz [Wdrażanie aplikacji za pomocą szablonu Menedżera zasobów Azure](resource-group-template-deploy.md).
