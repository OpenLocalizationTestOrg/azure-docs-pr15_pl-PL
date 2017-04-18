<properties
    pageTitle="Tworzenie nazw koncentratory wydarzenia z Centrum zdarzeń i Włącz archiwum przy użyciu szablonu Menedżera zasobów Azure | Microsoft Azure"
    description="Tworzenie nazw koncentratory wydarzenia z Centrum zdarzeń i Włącz archiwum przy użyciu szablonu Azure Menedżera zasobów"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Tworzenie nazw koncentratory zdarzenia z Centrum zdarzeń i Włącz archiwum przy użyciu szablonu Azure Menedżera zasobów

W tym artykule przedstawiono sposób użycia szablonu Menedżer zasobów Azure tworzy nazw koncentratory wydarzenia z koncentratora zdarzenia, które umożliwia archiwum na Twoim Centrum zdarzenia. Możesz dowiedzieć się, jak Aby zdefiniować zasoby, które są rozmieszczane i sposobu definiowania parametry, które są określone po wykonaniu wdrożenia. Użyj tego szablonu dla własnego wdrożenia lub Dostosuj go zgodnie z potrzebami

Aby uzyskać więcej informacji na temat tworzenia szablonów zobacz [Szablony do tworzenia Azure Menedżera zasobów][].

Więcej informacji na temat ćwiczeń i desenie konwencje nazewnictwa Azure zasobów można znaleźć [Konwencje nazewnictwa zasoby Azure][].

Zakończenie szablonu zobacz [Centrum zdarzeń i Włącz archiwum szablonu][] na GitHub.

>[AZURE.NOTE]
>Aby sprawdzić dostępność najnowszej szablony, odwiedź stronę galerii [Szablonów Szybki Start Azure][] i wyszukaj koncentratory zdarzenia.

## <a name="what-you-deploy"></a>Możesz wdrożyć?

Za pomocą tego szablonu wdrażanie nazw koncentratory wydarzenia z Centrum zdarzeń i Włącz archiwum.

[Koncentratory zdarzenia](../event-hubs/event-hubs-what-is-event-hubs.md) to zdarzenie Usługa używana do dostarczania zdarzeń i telemetrycznego ingress Azure na ogromną skalę z krótki czas oczekiwania i wysoka niezawodność przetwarzania. Archiwum koncentratory zdarzenia pozwoli automatycznego dostarczania danych strumieniowych w swojej koncentratory wydarzenia z magazynem obiektów Blob platformy Azure wybranych przez użytkownika w określonym czasie lub interwał rozmiar wybrane.

Aby automatycznie uruchamiać wdrażanie, przycisk:

[![Wdrażanie Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry

Przy użyciu Menedżera zasobów Azure Definiowanie parametrów wartości, które chcesz określić po wdrożeniu szablonu. Szablon zawiera sekcję o nazwie `Parameters` zawierający wszystkie wartości parametru. Należy zdefiniować parametr te wartości, które różnią się na podstawie projektu, który instalujesz lub na podstawie środowiska, który instalujesz do. Nie Definiowanie parametrów do wartości, które zawsze nie zmieniają się. Każda wartość parametru jest używana w szablonie Aby zdefiniować zasoby, które są rozmieszczane.

Szablon określa następujące parametry.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Nazwa nazw koncentratory zdarzenia, aby utworzyć.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

Nazwa Centrum zdarzeń zdarzeń utworzony koncentratory nazw.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Liczba dni mają one zachowywane w Twoim Centrum zdarzenia. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>Liczba partycji

Liczba partycje w Twoim Centrum zdarzenia.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Włącz archiwum na Twoim Centrum zdarzenia.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

Format kodowania, można określić szeregować dane zdarzenia.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Przedział czasu, w którym archiwum rozpoczyna się archiwizowanie danych w magazynie obiektów blob platformy Azure.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

Interwał rozmiar, w którym archiwum rozpoczyna się archiwizowanie danych w magazynie obiektów blob platformy Azure.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Archiwum wymaga identyfikator zasobu konta miejsca do magazynowania, aby włączyć archiwum do odpowiedniej magazyn Azure.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

Kontener obiektów blob miejsce, w którym chcesz dane zdarzenie można zarchiwizować.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

Interfejs API wersji szablonu.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Zasoby do wdrożenia

Tworzy przestrzeń nazw typu **EventHubs**z koncentratora zdarzenia, umożliwiając archiwum.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Polecenia do uruchamiania wdrażania

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>Programu PowerShell

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Polecenie Azure

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Tworzenie szablonów Azure Menedżera zasobów]: ../resource-group-authoring-templates.md
[Szablony Azure Szybki Start]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Zasoby Azure konwencje nazewnictwa]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Centrum zdarzeń i Włącz archiwum szablonu]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
