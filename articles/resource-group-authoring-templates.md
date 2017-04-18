<properties
   pageTitle="Do tworzenia Azure szablony Menedżera zasobów | Microsoft Azure"
   description="Tworzenie szablonów Menedżera zasobów Azure wdrażania aplikacji Azure za pomocą deklaracyjnych składni JSON."
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
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Tworzenie szablonów Azure Menedżera zasobów

W tym temacie opisano strukturę szablonu Azure Menedżera zasobów. Prezentowane przy jego użyciu różnych sekcjach szablonu i właściwości, które są dostępne w tych sekcji. Szablon zawiera JSON i wyrażeń, których można użyć do utworzenia wartości dla wdrożenia. 

Aby wyświetlić szablon dla zasobów, które zostały już rozmieszczone, zobacz [Eksportowanie szablonu Menedżera zasobów Azure z istniejących zasobów](resource-manager-export-template.md). Aby uzyskać instrukcje dotyczące tworzenia szablonu zobacz [Instruktaż szablonu Menedżera zasobów](resource-manager-template-walkthrough.md). Aby uzyskać zalecenia dotyczące tworzenia szablonów zobacz [Najważniejsze wskazówki dotyczące tworzenia szablonów Azure Menedżera zasobów](resource-manager-template-best-practices.md).

Warto edytora JSON może uprościć zadanie tworzenia szablonów. Aby dowiedzieć się, jak przy użyciu programu Visual Studio z szablonów zobacz [Tworzenie i wdrażanie grupy zasobów Azure za pomocą programu Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Aby dowiedzieć się, jak za pomocą w PORÓWNANIU z kodu zobacz [Praca z szablonów Menedżera zasobów Azure w kodzie programu Visual Studio](resource-manager-vs-code.md).

Ograniczyć rozmiar szablon 1 MB i każdy plik parametru 64 KB. Limit 1 MB dotyczy stan końcowy szablonu po została rozszerzona z definicji iteracyjne zasobów i wartościami zmiennych i parametrów. 

## <a name="template-format"></a>Formatowanie szablonu

W jego strukturę najłatwiejszym szablon zawiera następujące elementy:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Nazwa elementu   | Wymagane | Opis
| :------------: | :------: | :----------
| $schema        |   Tak    | Lokalizacja pliku schematu JSON opisującą wersją języka szablonu. Użyj adresu URL w poprzednim przykładzie.
| contentVersion |   Tak    | Wersja szablonu (na przykład 1.0.0.0). Można wprowadzać wartości dla tego elementu. Wdrażając zasobów za pomocą tego szablonu, ta wartość umożliwia upewnij się, że jest używany odpowiedni szablon.
| Parametry     |   Brak     | Wartości, które znajdują się podczas wdrażania jest wykonywane dostosowywanie wdrożenia zasobów.
| zmienne      |   Brak     | Wartości, które są używane jako fragmenty JSON w szablonie w celu uproszczenia wyrażenia języka szablonu.
| zasoby      |   Tak    | Typów zasobów, które są wdrażane lub zaktualizowany w grupie zasobów.
| Wyświetla        |   Brak     | Wartości, które są zwracane po wdrożeniu.

Firma Microsoft bada części szablonu bardziej szczegółowo w dalszej części tego tematu. Teraz możemy przejrzeć część składni, które tworzą szablonu.

## <a name="expressions-and-functions"></a>Funkcje i wyrażeń

Podstawowa składnia szablonu jest JSON. Jednak wyrażeń i funkcji rozszerzyć JSON, które są dostępne w szablonie. Przy użyciu wyrażeń możesz utworzyć wartości, które nie są ściśle literałów. Wyrażenia są ujęte w nawiasy kwadratowe [i] i są obliczane po wdrożeniu szablonu. Wyrażenia mogą występować w dowolnym miejscu wartość ciągu JSON i zawsze zwraca inną wartość JSON. Jeśli należy użyć literałów ciąg, który zaczyna się od nawias kwadratowy [, należy użyć dwóch nawiasy kwadratowe [[.

Zazwyczaj umożliwia wyrażenia za pomocą funkcji wykonywania operacji konfigurowania wdrożenia. Podobnie jak w JavaScript, funkcja połączeń zostały sformatowane jako **functionName(arg1,arg2,arg3)**. Właściwości odwoływać się przy użyciu operatorów kropki i [Indeks].

W poniższym przykładzie pokazano sposób używania funkcji kilka podczas tworzenia wartości:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Aby uzyskać pełną listę funkcji szablonu zobacz [funkcje szablonu Azure Menedżera zasobów](resource-group-template-functions.md). 


## <a name="parameters"></a>Parametry

W sekcji Parametry szablonu, należy określić wartości, które może wprowadzić wdrażając zasobów. Te wartości parametru umożliwiają dostosowywanie rozmieszczenia, dostarczając wartości, które są dostosowane dla określonego środowiska (na przykład deweloperów, test i produkcji). Nie musisz podać parametry w szablonie, ale bez parametrów szablonu będzie zawsze wdrażanie tych samych zasobów o tej samej nazwy, lokalizacje i właściwości.

Te wartości parametrów w szablonie służy do ustawiania wartości dla wdrożonej zasobów. Tylko parametry, które są zadeklarowane w sekcji Parametry można użyć w innych sekcji szablonu.

Definiowanie parametrów z następującą strukturę:

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| Nazwa elementu   | Wymagane | Opis
| :------------: | :------: | :----------
| Nazwa parametru  |   Tak    | Nazwa parametru. Musi być prawidłowym identyfikatorem języka JavaScript.
| Typ           |   Tak    | Typ wartości parametru. Zobacz listę poniżej dozwolonych typów.
| Wartość domyślna   |   Brak     | Wartość domyślna parametru, jeśli jest podana żadna wartość parametru.
| allowedValues  |   Brak     | Tablica dozwolona wartość parametru do należy się upewnić, że wartość prawej strony jest.
| wartość minValue       |   Brak     | Wartość minimalną parametrów typu int, ta wartość jest włącznie.
| wartość maxValue       |   Brak     | Maksymalna wartość dla parametrów typu int, ta wartość jest włącznie.
| minLength      |   Brak     | Minimalną długość ciągu, secureString i parametrów typ tablicy, ta wartość jest włącznie.
| maxLength      |   Brak     | Maksymalna długość ciągu, secureString i parametrów typ tablicy, ta wartość jest włącznie.
| Opis    |   Brak     | Opis parametr, który jest wyświetlany użytkownikom szablonu za pośrednictwem interfejsu portalu szablon niestandardowy.

Dozwolonych typów i wartości są:

- **ciąg**
- **secureString**
- **int**
- **wartość logiczna**
- **obiekt** 
- **secureObject**
- **Tablica**

Aby określić parametr jako opcjonalne, podaj wartość domyślna (może być ciąg pusty). 

Jeśli użytkownik określi nazwę parametru, który pasuje do jednej z parametrów w poleceniu wdrożyć szablonu, zostanie wyświetlony monit o podanie wartości parametru z przyrostek **FromTemplate**. Na przykład jeśli zawiera parametr o nazwie **ResourceGroupName** w szablonie jest taka sama, jak parametr **ResourceGroupName** w [AzureRmResourceGroupDeployment nowy] [ deployment2cmdlet] polecenia cmdlet, zostanie wyświetlony monit o podanie wartości dla **ResourceGroupNameFromTemplate**. Ogólnie należy unikać ten błąd, nie nadając parametrów taką samą nazwę jak parametry używane do operacji rozmieszczania.

>[AZURE.NOTE] Wszystkie hasła, klucze i inne hasła należy stosować typ **secureString** . Nie można odczytać parametrów szablonu z typem secureString po wdrożeniu zasobów. 

W poniższym przykładzie pokazano sposób Definiowanie parametrów:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Jak wartości parametrów wejściowych podczas wdrażania, zobacz temat [Deploy aplikacji za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md#parameter-file). 

## <a name="variables"></a>Zmienne

W sekcji Zmienne utworzeniem wartości, które mogą być używane w szablonie. Zazwyczaj zmiennych na podstawie wartości z parametrów. Nie należy zdefiniować zmienne, ale są często uprościć szablonu zmniejszając wyrażeniach złożonych.

Definiowanie zmiennych z następującą strukturę:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

W poniższym przykładzie pokazano sposób definiowania zmienna, która jest tworzona z dwóch wartości parametru:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

W kolejnym przykładzie zmiennej będącej złożony JSON i zmiennych, które są wykonane z innych czynników:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Zasoby

W sekcji Zasoby do definiowania zasoby, które wdrożony lub aktualizacji. W tej sekcji można uzyskać skomplikowane, ponieważ należy zrozumieć typów, który instalujesz zapewnienie właściwych wartości. 

Definiowanie zasoby z następującą strukturę:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Nazwa elementu             | Wymagane | Opis
| :----------------------: | :------: | :----------
| apiVersion               |   Tak    | Wersja interfejsu API usługi REST użyty do utworzenia zasobu.
| Typ                     |   Tak    | Typ zasobu. Ta wartość jest kombinacją nazw dostawcy zasobów i typ zasobu (na przykład **Microsoft.Storage/storageAccounts**).
| Nazwa                     |   Tak    | Nazwa zasobu. Nazwa musi być zgodna ograniczeń składnika URI w RFC3986. Ponadto usług Azure, z użyciem nazwę zasobu, aby poza strony Sprawdź nazwę, aby upewnić, że nie jest próba sfałszować tożsamość. Zobacz, [Sprawdź nazwę zasobu](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| Lokalizacja                 |   Zmienia się  | Obsługiwane geo lokalizacje dostarczonych zasobów. Można wybrać dowolny z dostępnych lokalizacji, ale zwykle jest to właściwe rozwiązanie do wybierz jedną z nich podobny do takiego użytkownika. Zazwyczaj również jest to właściwe rozwiązanie, aby umieścić zasoby, które współdziałać ze sobą w tym samym regionie. Większość typów zasobów wymagają lokalizacji, ale niektóre typy (na przykład przypisanie roli) wymagają lokalizacji.
| znaczniki                     |   Brak     | Znaczniki, które są skojarzone z zasobem.
| komentarze                 |   Brak     | Notatki na dokumentowanie zasobów w szablonie
| dependsOn                |   Brak     | Zasoby, które zależy od tego zasobu został określony. Zależności między zasoby są szacowane i ich kolejnością zależne są wdrażane zasoby. Gdy zasoby są zależne od siebie, są oni wdrażane równolegle. Wartość może być rozdzielaną średnikami listę zasobu nazwy lub unikatowych identyfikatorów zasobów.
| właściwości               |   Brak     | Ustawienia specyficzne dla zasobów. Wartości właściwości są takie same jak wartości, które zapewniają w treści wezwania do działania interfejsu API usługi REST (metoda położenie) do utworzenia zasobu. Łącza do zasobów schematu dokumentacji lub interfejsu API usługi REST zobacz [Menedżer zasobów dostawców, regionów, wersje interfejsu API i schematów](resource-manager-supported-services.md).
| Kopiuj                     |   Brak     | W razie potrzeby więcej niż jedno wystąpienie liczby zasobów, aby utworzyć. Aby uzyskać więcej informacji zobacz [Tworzenie wielu wystąpień zasobów w Menedżerze zasobów Azure](resource-group-create-multiple.md). |
| zasoby                |   Brak     | Zasoby podrzędne, które zależą od zasobu został określony. Można udostępnić tylko typy zasobów, które są dozwolone w schemacie zasobów nadrzędnej. W pełni kwalifikowana nazwa typu zasobu podrzędne zawiera nadrzędny typ zasobu, takie jak **Microsoft.Web/sites/extensions**. Zależność od zasobów nadrzędnego nie jest oznaczany; Zależność ta jawnie należy zdefiniować. 

Informacje o co wartości, aby określić dla **apiVersion**, **typu**i **lokalizacji** nie jest od razu oczywiste. Na szczęście można określić następujące wartości za pomocą programu PowerShell Azure lub polecenie Azure.

Aby uzyskać wszystkich dostawców zasobów przy użyciu **programu PowerShell**, należy użyć:

    Get-AzureRmResourceProvider -ListAvailable

Na liście zwróconych Znajdź dostawców zasobów, który Cię interesuje. Aby uzyskać typów zasobów dla dostawcy zasobów (na przykład miejsce do magazynowania), należy użyć:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Aby uzyskać wersji interfejsu API dla typu zasobu (takiego konta miejsca do magazynowania), należy użyć:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Aby uzyskać obsługiwanych lokalizacje typ zasobu, należy użyć:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

Aby uzyskać wszystkich dostawców zasobów z **Polecenie Azure**, należy użyć:

    azure provider list

Na liście zwróconych Znajdź dostawców zasobów, który Cię interesuje. Aby uzyskać typów zasobów dla dostawcy zasobów (na przykład miejsce do magazynowania), należy użyć:

    azure provider show Microsoft.Storage

Aby uzyskać obsługiwanych lokalizacji i wersje interfejsu API, użyj:

    azure provider show Microsoft.Storage --details --json

Aby dowiedzieć się więcej na temat dostawców zasobów, zobacz [Menedżer zasobów dostawców, regionów, wersje interfejsu API i schematów](resource-manager-supported-services.md).

W sekcji zasobów zawiera tablicę zasobów do wdrożenia. W obrębie każdego zasobu można także zdefiniować tablicę podrzędne zasobów. Strukturę może więc sekcji Zasoby:

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


W poniższym przykładzie pokazano zasoby **Microsoft.Web/serverfarms** i **Microsoft.Web/sites** z dzieckiem **rozszerzenia** zasobu. Zwróć uwagę, że witryny został oznaczony jako zależne od farmy serwerów, ponieważ farmy serwerów, musi istnieć witryny mogą być rozmieszczone. Zbyt należy zauważyć, że zasobów **rozszerzenia** jest podrzędnego witryny.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Wyjściowe

W sekcji wyjściowe można określić wartości, które są zwracane z wdrożenia. Na przykład może zwrócić identyfikatora URI uzyskiwać dostęp do wdrożonym zasobu.

W poniższym przykładzie pokazano strukturę definicji danych wyjściowych:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Nazwa elementu   | Wymagane | Opis
| :------------: | :------: | :----------
| outputName     |   Tak    | Nazwa wartości wyjściowych. Musi być prawidłowym identyfikatorem języka JavaScript.
| Typ           |   Tak    | Typ wartości wyjściowych. Wartości wyjściowych obsługuje samego typu co szablon parametrów wejściowych.
| wartość          |   Tak    | Wyrażenie języka szablonu, które jest obliczane i zwrócony jako wartość wyjściową.


W poniższym przykładzie pokazano wartość, która jest zwracana w sekcji wyjściowe.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Aby uzyskać więcej informacji na temat pracy z dane wyjściowe zobacz [Udostępnianie stan w szablonach Azure Menedżera zasobów](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Następne kroki
- Aby wyświetlić pełną szablony do różnych rozwiązań, zobacz [Szablony Szybki Start Azure](https://azure.microsoft.com/documentation/templates/).
- Aby uzyskać szczegółowe informacje o funkcjach, których można używać z poziomu szablonu zobacz [Funkcje szablonu Menedżera zasobów Azure](resource-group-template-functions.md).
- Aby połączyć wiele szablonów podczas wdrażania, zobacz [Korzystanie z szablonów połączone z Menedżerem zasobów Azure](resource-group-linked-templates.md).
- Konieczne może być zasoby, które występują w różnych grupach zasobów. W tym scenariuszu jest typowych podczas pracy z kontami miejsca do magazynowania lub wirtualnych sieci udostępnianych w przypadku wielu grup zasobów. Aby uzyskać więcej informacji zobacz temat [Funkcja resourceId](resource-group-template-functions.md#resourceid).


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx
