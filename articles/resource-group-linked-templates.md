<properties
   pageTitle="Połączone szablonów przy użyciu Menedżera zasobów | Microsoft Azure"
   description="Opisano, jak utworzyć rozwiązanie moduły szablon przy użyciu szablonów połączone w szablonie Azure Menedżera zasobów. Pokazano, jak przekazać wartości parametrów, określ plik parametru i dynamicznie utworzony adresy URL."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Korzystanie z szablonów połączone z Menedżerem zasobów Azure

Z w jednym szablonie Azure Menedżera zasobów, można połączyć innego szablonu, który umożliwia których przedstawiany wdrożenia na zbiór przeznaczona, szablony określonego celu. Podobnie jak w przypadku decomposing aplikacji do kilku klas kodu, dekompozycji zalety badania, ponowne użycie i czytelności.  

Można przekazać do szablonu połączonego parametry z szablonu głównym, a tych parametrów bezpośrednio można mapować na parametry lub zmiennych w szablonie połączeń. Szablonu połączonego można również przekazać zmienną wyjściową do szablonu źródła Włączanie dwukierunkowe danych wymiany szablonów.

## <a name="linking-to-a-template"></a>Tworzenie łączy do szablonu

Tworzenie łącza między dwoma szablony, dodając zasobu wdrożenia w głównym szablonu, która kieruje do szablonu połączonego. Właściwość **templateLink** Ustaw identyfikator URI szablonu połączonego. Wartości parametrów można udostępnić dla szablonu połączonego, określając wartości bezpośrednio w szablonie lub za pomocą łączenia z pliku parametrów. W poniższym przykładzie użyto właściwości **Parametry** , aby określić wartości parametru bezpośrednio.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Usługa Menedżera zasobów muszą mieć możliwość dostępu do połączonych szablonu. Nie można określić pliku lokalnego lub plik, który jest dostępna tylko w sieci lokalnej dla szablonu połączonego. Tylko pozwalają wartość identyfikatora URI, która zawiera **http** lub **https**. Jedną z opcji jest umieść szablonu połączonego z konta miejsca do magazynowania, a następnie użyj identyfikatora URI dla tego elementu, tak jak pokazano w poniższym przykładzie.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Mimo że szablonu połączonego musi być dostępny zewnętrznie, nie powinien być powszechnie dostępny publicznie. Możesz dodać do szablonu na konto prywatne miejsca do magazynowania, które będzie dostępne dla tylko właściciel konta miejsca do magazynowania. Następnie można utworzyć tokenu podpisu (SA) udostępnienia włączenia dostępu podczas wdrażania. Możesz dodać token skojarzenia zabezpieczeń do identyfikatora URI dla szablonu połączonego. Instrukcje dotyczące Konfigurowanie szablonu z konta magazynu i generowanie tokenu skojarzeń zabezpieczeń zobacz [zasoby rozmieszczanie z szablonami Menedżera zasobów i polecenie Azure](resource-group-template-deploy-cli.md)lub [Rozmieszczanie zasoby z szablonami Menedżera zasobów i Azure programu PowerShell](resource-group-template-deploy.md) . 

W poniższym przykładzie pokazano szablonu nadrzędnego z łączem do innego szablonu. Szablonu połączonego jest dostępna z tokenu skojarzeń zabezpieczeń, który jest przekazywany jako parametru.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Mimo że tokenu przekazywana w postaci ciągu bezpiecznego, identyfikator URI połączonych szablonu, w tym token skojarzenia zabezpieczeń są rejestrowane w operacji rozmieszczania dla danej grupy zasobów. Aby ograniczyć narażenie, ustaw wygaśnięcia dla tokenu.

## <a name="linking-to-a-parameter-file"></a>Łączenie z plikiem parametru

W następnym przykładzie użyto właściwości **parametersLink** , aby utworzyć łącze do pliku parametrów.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

Wartość identyfikatora URI pliku połączonego parametru nie może być plik lokalny, a może zawierać **http** lub **https**. Pliku parametrów można także ograniczone do programu access za pomocą tokenu skojarzeń zabezpieczeń.

## <a name="using-variables-to-link-templates"></a>Łącze Szablony przy użyciu zmiennych

Poprzednich przykładach pokazano ustalonych wartości adres URL łącza szablonu. Tej metody może działać dla szablonu proste, ale nie działa w przypadku pracy z dużym zestawem modułowych szablonów. Zamiast tego można utworzyć zmienną statyczne przechowującego podstawowy adres URL szablonu głównym, a następnie dynamicznie utworzyć adresy URL dla szablonów połączonych z tym podstawowy adres URL. Zaletą tej metody jest można łatwo przenosić i rozwidlenia szablonu, ponieważ musisz zmienić zmienną statyczne w szablonie głównym. Szablon głównym przekazuje prawidłowe identyfikatory URI w szablonie rozłożony.

W poniższym przykładzie pokazano sposób użycia podstawowy adres URL do tworzenia dwóch adresów URL dla połączonej szablonów (**sharedTemplateUrl** i **vmTemplate**). 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Można też użyć [deployment()](resource-group-template-functions.md#deployment) , aby uzyskać podstawowy adres URL dla bieżącego szablonu i używać, aby uzyskać adres URL dla innych szablonów w tej samej lokalizacji. Ta metoda jest przydatna, jeśli zmieni się lokalizację szablonu (być może ze względu na przechowywanie wersji) lub chcesz uniknąć trwałym kodowania adresy URL w pliku szablonu. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Warunkowe łączenie z szablonów

Można połączyć z różnych szablonów, przy przekazanie wartości parametru, który służy do tworzenia identyfikator URI szablonu połączonego. Ta metoda działa również w przypadku, gdy trzeba określić podczas wdrażania, które połączone szablon do użycia. Na przykład można określić szablon, aby użyć dla istniejącego konta miejsca do magazynowania i innego szablonu dla nowego konta miejsca do magazynowania.

W poniższym przykładzie pokazano parametru nazwę konta magazynu i określ, czy konto miejsca do magazynowania jest nowym lub istniejącym parametr.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Tworzenie zmiennej identyfikatora URI, która zawiera wartość parametru nowym lub istniejącym szablonu.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Ta wartość zmiennej przewidzieć zasobów wdrożenia.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

Identyfikator URI jest rozpoznawana jako szablon o nazwie **existingStorageAccount.json** lub **newStorageAccount.json**. Tworzenie szablonów dla tych identyfikatory URI.

W poniższym przykładzie pokazano szablon **existingStorageAccount.json** .

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

W kolejnym przykładzie szablonu **newStorageAccount.json** . Zwróć uwagę, że takich jak magazyn istniejący szablon konta obiekt konta magazynu jest zwracana w wyjściowe. Szablonu współdziała z obu szablonu połączonego.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Rozbudowany przykład

Przykład szablonach Pokaż uproszczone rozmieszczenia połączonych szablonów aby przedstawić niektóre pojęcia w tym artykule. Przyjęto założenie, że szablony zostały dodane do samej kontenera na koncie miejsca do magazynowania o dostępie wyłączone. Szablonu połączonego przekazuje wartość z powrotem na głównym szablonu w sekcji **Wyświetla** .

Plik **parent.json** zawiera:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Plik **helloworld.json** zawiera:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
W programie PowerShell możesz uzyskać token dla kontenera i wdrażanie szablonów:

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

Polecenie Azure służy do uzyskania tokenu kontenera i wdrażanie szablonów z poniższy kod. Obecnie musisz podać nazwę wdrożenia podczas korzystania z szablonu identyfikator URI, który zawiera token skojarzeń zabezpieczeń.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Zostanie wyświetlony monit o podanie token skojarzeń zabezpieczeń jako parametru. Należy poprzedzić token z **?**.

## <a name="next-steps"></a>Następne kroki
- Aby dowiedzieć się o definiowaniu kolejności wdrożenia dla zasobów, zobacz [Definiowanie zależności w szablonach Azure Menedżera zasobów](resource-group-define-dependencies.md)
- Aby dowiedzieć się, jak zdefiniować jeden zasób, ale utworzenia wielu wystąpień, zobacz [Tworzenie wielu wystąpień zasobów w Menedżerze zasobów Azure](resource-group-create-multiple.md)
