<properties
    pageTitle="Eksportuj szablon Menedżera zasobów Azure | Microsoft Azure"
    description="Umożliwia zarządzanie zasobów Azure Eksportowanie szablonu z istniejącej grupy zasobów."
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
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="tomfitz"/>

# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Eksportowanie szablonu Menedżera zasobów Azure z istniejących zasobów

Menedżer zasobów umożliwia eksportowanie szablonu Menedżera zasobów z istniejących zasobów w ramach subskrypcji. Skorzystać z tego szablonu wygenerowanym, aby uzyskać informacje o składni szablonu lub aby zautomatyzować ponownego wdrażania rozwiązania, stosownie do potrzeb.

Należy pamiętać, że istnieją dwa różne sposoby eksportowania szablonu:

- Możesz wyeksportować rzeczywisty szablon, którego użyto do wdrożenia. Wyeksportowane szablon zawiera wszystkie parametry i zmienne dokładnie tak, jak były wyświetlane na oryginalnym szablonie. Ta metoda jest przydatne, gdy wdrożono zasobów za pośrednictwem portalu. Teraz chcesz dowiedzieć się, jak utworzyć szablon do tworzenia tych zasobów.
- Możesz wyeksportować szablonu, który odpowiada bieżącemu stanowi grupa zasobów. Wyeksportowane szablon nie jest oparty na dowolny szablon, którego użyto do wdrożenia. Zamiast tego tworzy szablon migawkę grupy zasobów. Wyeksportowane szablon zawiera wiele wartości stałe i prawdopodobnie nie dowolną liczbę parametrów jak zwykle należy zdefiniować. Ta metoda jest przydatna, modyfikacji grupa zasobów za pośrednictwem portalu lub skryptów. Teraz należy przechwycić grupa zasobów jako szablon.

W tym temacie opisano obie metody.

W tym samouczku możesz zalogować się do portalu Azure, Utwórz konto miejsca do magazynowania, a Eksportowanie szablonu dla tego konta miejsca do magazynowania. Możesz dodać wirtualnej sieci do modyfikowania grup zasobów. Na koniec możesz wyeksportować nowego szablonu, który odpowiada jego bieżącym stanie. Mimo że w tym artykule omówiono uproszczone infrastruktury, można użyć te same czynności do wyeksportowania szablonu dla bardziej złożone rozwiązanie.

## <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

1. W [portalu Azure](https://portal.azure.com), wybierz pozycję **Nowy** > **miejsca do magazynowania** > **konta miejsca do magazynowania**.

      ![Tworzenie miejsca do magazynowania](./media/resource-manager-export-template/create-storage.png)

2. Utwórz konto miejsca do magazynowania z nazwy **miejsca do magazynowania**, inicjały i daty. Nazwę konta magazynu musi być unikatowa we Azure. Jeśli nazwa jest już używana, pojawi się komunikat o błędzie wskazujący, że nazwa jest używana. Spróbuj odmiany. Dla grupy zasobów tworzenie nowej grupy zasobów i nadaj mu nazwę **ExportGroup**. Za pomocą wartości domyślnych dla innych właściwości. Wybierz polecenie **Utwórz**.

      ![Podaj wartości dla miejsca do magazynowania](./media/resource-manager-export-template/provide-storage-values.png)

Wdrażanie może potrwać minutę. Po zakończeniu rozmieszczania subskrypcji zawiera konto miejsca do magazynowania.

## <a name="view-a-template-from-deployment-history"></a>Przeglądanie szablonów z historii rozmieszczania

1. Przejdź do karta Grupa zasobów dla nowej grupy zasobów. Zwróć uwagę, że karta Podgląd wyniku ostatniego wdrożenia. Wybierz to łącze.

      ![Karta Grupa zasobów](./media/resource-manager-export-template/resource-group-blade.png)

2. Zobacz Historia wdrożeń grupy. W Twoim przypadku karta prawdopodobnie lista tylko jeden wdrożenia. Zaznacz ten wdrożenia.

     ![ostatniego wdrożenia](./media/resource-manager-export-template/last-deployment.png)

3. Karta zawiera zestawienie wdrożenia. Podsumowanie zawiera stan wdrożenia i jego operacji i wartości, które podany dla parametrów. Aby wyświetlić szablon, którego użyto do wdrożenia, wybierz **Szablon widoku**.

     ![Wyświetl podsumowanie wdrażania](./media/resource-manager-export-template/deployment-summary.png)

4. Menedżer zasobów pobiera następujące pliki sześć można:

   1. **Szablon** — szablon, który definiuje infrastruktury dla tego rozwiązania. Po utworzeniu konta miejsca do magazynowania za pośrednictwem portalu Menedżer zasobów umożliwia wdrożeniem tego szablonu i zapisać ten szablon do użytku w przyszłości.
   2. **Parametry** - plik parametr, który można użyć w celu przekazania wartości podczas wdrażania. Zawiera wartości, które podany podczas pierwszego wdrażania, ale można zmienić dowolne z tych wartości, gdy zostanie ponownie wdróż szablonu.
   3. **Polecenie** - Azure polecenia wiersza interface (polecenie) skryptu pliku, który umożliwia wdrożyć szablonu.
   4. **PowerShell** — plik skryptu Azure programu PowerShell służące do wdrożenia szablonu.
   5. **.NET** — klasy .NET, która umożliwia wdrożyć szablonu.
   6. **Dopiskiem** - klasy dopiskiem, która umożliwia wdrożyć szablonu.

     Pliki są dostępne za pośrednictwem łączy przez karta. Domyślnie karta zostanie wyświetlony szablon.

       ![Wyświetl szablon](./media/resource-manager-export-template/view-template.png)

     Załóżmy zwrócić szczególną uwagę do szablonu. Szablon powinna wyglądać podobnie do:

        {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "Parametry": {"Nazwa": {"typ": "Ciąg"}, "dla konta": {"typ": "Ciąg"}, "położenie": {"typ": "Ciąg"}, "encryptionEnabled": {"wartość domyślna": ma wartość FAŁSZ, "typ": "Wartość logiczna"}}, "zasobów": [{"typ": "Microsoft.Storage/storageAccounts", "sku": {"Nazwa": "[parameters('accountType')]"}, "rodzaju": "Miejscem do magazynowania", "Nazwa": "[parameters('name')]", "apiVersion": "2016-01-01", "położenie": "[parameters('location')]", "właściwości": {"szyfrowania": {"usług": {"obiektów blob": {"włączono": "[parameters('encryptionEnabled')]"}}, "keySource": "Microsoft.Storage"}}}]}
 
Ten szablon jest rzeczywisty szablon użyte do utworzenia konta miejsca do magazynowania. Zwróć uwagę, że zawiera ona parametry, które umożliwiają wdrażanie różne typy kont miejsca do magazynowania. Aby dowiedzieć się więcej o strukturze szablonu, zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md). Aby uzyskać pełną listę funkcji, których można używać w szablonie zobacz [funkcje szablonu Azure Menedżera zasobów](resource-group-template-functions.md).


## <a name="add-a-virtual-network"></a>Dodawanie wirtualnej sieci

Szablon, który został pobrany w poprzedniej sekcji reprezentowane infrastruktury do tego oryginalny wdrożenia. Jednak mogą nie uwzględniać wszelkie zmiany wprowadzone po wdrożeniu.
Aby przedstawić ten problem, Przejdźmy zmodyfikować grupa zasobów, dodając wirtualną sieć za pośrednictwem portalu.

1. W karta Grupa zasobów wybierz pozycję **Dodaj**.

      ![Dodawanie zasobu](./media/resource-manager-export-template/add-resource.png)

2. Wybierz pozycję **wirtualnej sieci** z dostępnych zasobów.

      ![Wybierz pozycję wirtualnej sieci](./media/resource-manager-export-template/select-vnet.png)

2. Nadaj nazwę sieci wirtualnej **VNET**i użyć wartości domyślnych dla innych właściwości. Wybierz polecenie **Utwórz**.

      ![Ustaw alert](./media/resource-manager-export-template/create-vnet.png)

3. Po wirtualnej sieci został pomyślnie wdrożony do grupy zasobów, sprawdź ponownie historii wdrożenia. Teraz zobaczyć dwóch wdrożenia. Jeśli druga wdrożenia nie jest widoczna, może być konieczne zamknięcie programu karta Grupa zasobów i ponownym otwarciu dokumentu. Wybierz najnowszą wdrożenia.

      ![Historia wdrażania](./media/resource-manager-export-template/deployment-history.png)

4. Przeglądanie szablonów do tego wdrożenia. Zwróć uwagę, że definiuje tylko wirtualnej sieci. Nie ma konta miejsca do magazynowania, które wcześniej wdrożono. Masz już szablonu, która reprezentuje wszystkie zasoby w grupie zasobów.

## <a name="export-the-template-from-resource-group"></a>Eksportowanie szablonu z Grupa zasobów

Aby uzyskać bieżący stan grupy zasobów, eksportowanie szablonu, który zawiera migawkę grupy zasobów.  

> [AZURE.NOTE] Nie można eksportować szablonu dla grupa zasobów, która zawiera więcej niż 200 zasobów.

1. Aby wyświetlić szablonu dla grupy zasobów, zaznacz **skrypt automatyzacji**.

      ![Eksportowanie grupa zasobów](./media/resource-manager-export-template/export-resource-group.png)

     Nie we wszystkich typach zasobów pomocy technicznej funkcji szablonu eksportu. Jeśli grupy zasobów zawiera tylko konta miejsca do magazynowania i wirtualną sieć przedstawione w tym artykule, nie ma komunikat o błędzie. Jednak jeśli użytkownik utworzył innych typów zasobów, zobaczysz komunikat o błędzie informujący, że występuje problem z Eksportuj. Jak obsługiwać te problemy w sekcji [Rozwiąż problemy eksportu](#fix-export-issues) .

      

2. Ponownie wyświetlić sześć pliki, które mogą być ponownie wdrożyć rozwiązanie, ale tym razem szablon jest nieco inaczej. Ten szablon ma tylko dwa parametry: jedną dla nazwę konta magazynu i jedną dla nazwy wirtualnej sieci.

        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },

     Menedżer zasobów nie pobrać szablony, które zostały użyte podczas wdrażania. Zamiast tego wygenerowanie nowego szablonu, który jest na podstawie bieżącej konfiguracji zasobów. Na przykład szablon ustawia wartość lokalizację i replikacji konta miejsca do magazynowania:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Istnieje kilka opcji dotyczących kontynuowanie pracy za pomocą tego szablonu. Można pobrać szablon i pracować nad nim lokalnie w edytorze JSON. Lub możesz zapisać szablon do biblioteki i pracować nad nim w portalu.

     Jeśli uważasz, przy użyciu edytora JSON, takich jak [Kod w PORÓWNANIU z](resource-manager-vs-code.md) lub [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), można wybrać pobraniu szablonu lokalnie i przy użyciu tego edytora. Jeśli nie są skonfigurowane przy użyciu edytora JSON, można wybrać edytowanie szablonu za pośrednictwem portalu. Pozostała część tego tematu założono, że szablon został zapisany w bibliotece w portalu. Jednak zmiany samej składni w szablonie zarówno lokalnie pracy za pomocą edytora JSON lub za pośrednictwem portalu.

     Aby pracować lokalnie, wybierz pozycję **Pobieranie**.

      ![Pobieranie szablonu](./media/resource-manager-export-template/download-template.png)

     Aby pracować za pośrednictwem portalu, wybierz pozycję **Dodaj do biblioteki**.

      ![Dodawanie do biblioteki](./media/resource-manager-export-template/add-to-library.png)

     Podczas dodawania szablonu do biblioteki, Nadaj szablonowi nazwę i opis. Następnie wybierz pozycję **Zapisz**.

     ![Ustawianie szablonu wartości](./media/resource-manager-export-template/set-template-values.png)

4. Szablon zapisany w bibliotece zaznacz, aby wyświetlić **więcej usług**, wpisz **Szablony** do filtrowania wyników, wybierz pozycję **Szablony**.

      ![znajdowanie szablonów](./media/resource-manager-export-template/find-templates.png)

5. Wybierz szablon o nazwie, który został zapisany.

      ![Wybierz szablon](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>Dostosowywanie szablonu

Wyeksportowanych szablonów działa prawidłowo, jeśli chcesz utworzyć tego samego konta miejsca do magazynowania i wirtualnych sieci każdej wdrożenia. Jednak Menedżer zasobów udostępnia opcje, dzięki czemu można wdrażać szablony znacznie większą elastyczność. Na przykład podczas wdrażania, można określić typ konta przestrzeni dyskowej, aby utworzyć lub wartości do używania prefiksu adresu wirtualnej sieci i prefiks podsieci.

W tej sekcji możesz dodać parametry wyeksportowanych szablonów, tak aby można było ponownie użyć szablonu, po wdrożeniu tych zasobów do innych środowisk. Niektóre funkcje można też dodać do szablonu, aby zmniejszyć prawdopodobieństwo wystąpieniu błędu po wdrożeniu szablonu. Nie masz już odgadnięcie unikatową nazwę konta magazynu. Zamiast tego szablonu tworzy unikatową nazwę. Ograniczanie wartości, które można określić typ konta miejsca do magazynowania, aby tylko prawidłowe opcje.

1. Wybierz pozycję **Edytuj** , aby dostosować szablon.

     ![Pokazywanie szablonu](./media/resource-manager-export-template/show-template.png)

1. Wybierz szablon.

     ![Edytowanie szablonu](./media/resource-manager-export-template/edit-template.png)

1. Aby mieć możliwość przekazywania wartości, które można określić podczas wdrażania, Zastąp sekcji **Parametry** nowych definicji parametru. Zwróć uwagę, wartości **allowedValues** dla **storageAccount_accountType**. Jeśli przypadkowo zawiera nieprawidłową wartość, że jest rozpoznawany przed rozpoczęciem wdrażania. Ponadto zauważyć, że tylko prefiksu mają być udostępniane dla nazwę konta magazynu oraz prefiks jest ograniczona do 11 znaków. Ograniczyć prefiks do 11 znaków, uważaj, że pełną nazwę nie przekracza maksymalną liczbę znaków dla konta miejsca do magazynowania. Prefiks umożliwia stosowanie konwencji nazewnictwa do konta miejsca do magazynowania. Zobaczysz jak utworzyć unikatową nazwę w następnym kroku.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. Sekcja **zmienne** szablonu jest obecnie pusta. W sekcji **zmienne** możesz utworzyć wartości, które uprościć składni dla każdego szablonu. W tej sekcji należy zastąpić nową definicję zmiennych. Zmienna **storageAccount_name** łączy prefiks parametr unikatowy ciąg, który jest generowany na podstawie identyfikatora grupy zasobów. Nie masz już odgadnięcie unikatową nazwę, udostępniając wartości parametru.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. Aby użyć parametrów i zmiennych w definicji zasobów, należy zastąpić sekcji **zasoby** nowych definicji zasobów. Należy zauważyć, że zmienił się nieco w definicji zasobów niż wartość, która jest przypisana do właściwości zasobów. Właściwości są takie same jako właściwości w szablonie wyeksportowane. Właściwości są przydzielane po prostu wartości parametru zamiast ustalonych wartości. Lokalizacja zasobów ustawiono jako grupa zasobów za pomocą wyrażenia **.location () grupa zasobów** za pomocą tej samej lokalizacji. Zmienna utworzonego dla nazwę konta magazynu odwołuje się za pomocą wyrażenia **zmiennych** .

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

4. Wybierz **przycisk OK** , po zakończeniu edycji szablonu.

5. Wybierz pozycję **Zapisz** , aby zapisać zmiany w szablonie.

     ![Zapisywanie szablonu](./media/resource-manager-export-template/save-template.png)

6. Aby wdrożyć uaktualnionego szablonu, wybierz pozycję **Deploy**.

     ![Wdrażanie szablonu](./media/resource-manager-export-template/deploy-template.png)

7. Podaj wartości parametru, a następnie wybierz nową grupę zasobów do zasobów w celu wdrożenia.

## <a name="update-the-downloaded-parameters-file"></a>Aktualizowanie parametrów pobranego pliku

Jeśli pracujesz z pobrane pliki (zamiast portalu biblioteki), musisz zaktualizować plik pobrany parametru. Nie odpowiada ona parametrów w szablonie. Nie musisz użyć parametru pliku, ale można uprościć proces, gdy zostanie ponownie wdróż środowisku. Użyj wartości domyślne, które są definiowane w szablonie dla wielu parametrów tak, aby plik parametr musi tylko dwie wartości.

Zamień zawartość pliku parameters.json:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Plik zaktualizowane parametr zawiera wartości tylko dla parametrów, które nie mają wartości domyślne. Można podać wartości inne parametry, gdy ma wartość inną niż wartość domyślna.

## <a name="fix-export-issues"></a>Rozwiązywanie problemów dotyczących eksportu

Nie we wszystkich typach zasobów pomocy technicznej funkcji szablonu eksportu. Menedżer zasobów nie specjalnie eksportować niektórych typów zasobów, aby zapobiec Uwidacznianie ważnych danych. Na przykład jeśli masz parametry połączenia w pliku config witryny, prawdopodobnie nie ma go jawnie wyświetlana w wyeksportowanych szablonów. Aby uzyskać rozwiązać ten problem, należy ręczne dodawanie Brak zasobów do szablonu.

> [AZURE.NOTE] Podczas eksportowania z grupy zasobów, a nie z historii wdrożenia tylko wystąpienia problemów eksportu. Jeśli ostatniego wdrożenia dokładnie odzwierciedla bieżący stan grupa zasobów, możesz wyeksportować szablonu z folderu Historia wdrażania, a nie z grupy zasobów. Po wprowadzeniu zmian do grupy zasobów, które nie są definiowane w jednym szablonu tylko wyeksportować z grupy zasobów.

Na przykład podczas eksportowania szablonu dla grupa zasobów, która zawiera aplikację sieci web, bazy danych SQL i parametry połączenia w konfiguracji witryny, pojawi się następujący komunikat:

![wyświetlenie błędu](./media/resource-manager-export-template/show-error.png)

Wybieranie wiadomość zawiera dokładnie typów zasobów, które nie zostały wyeksportowane. 
     
![wyświetlenie błędu](./media/resource-manager-export-template/show-error-details.png)

W tym temacie opisano typowe rozwiązania.

### <a name="connection-string"></a>Parametry połączenia

W zasobu witryn sieci web Dodaj definicję parametry połączenia z bazą danych:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### <a name="web-site-extension"></a>Rozszerzenie witryny sieci Web

W zasobu witryny sieci web Dodaj Definicja kodu instalacji:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### <a name="virtual-machine-extension"></a>Rozszerzenie maszyn wirtualnych

Aby zapoznać się z przykładami rozszerzeń maszyn wirtualnych zobacz [Przykłady konfiguracji rozszerzenia maszyn wirtualnych systemu Windows Azure](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### <a name="virtual-network-gateway"></a>Brama wirtualnej sieci

Dodawanie typu zasobu Brama wirtualnej sieci.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### <a name="local-network-gateway"></a>Brama sieci lokalnej

Dodawanie typu zasobu bramy sieci lokalnej.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### <a name="connection"></a>Połączenia

Dodawanie typu zasobu połączenia.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## <a name="next-steps"></a>Następne kroki

Gratulacje! Wiesz sposób eksportowania szablonu od zasobów, które zostały utworzone w portalu.

- Można wdrażać szablonu za pomocą [programu PowerShell](resource-group-template-deploy.md), [Polecenie Azure](resource-group-template-deploy-cli.md)lub [Interfejsu API usługi REST](resource-group-template-deploy-rest.md).
- Aby zobaczyć jak eksportować szablon przy użyciu programu PowerShell, zobacz [Za pomocą programu Azure przy użyciu Menedżera zasobów Azure](powershell-azure-resource-manager.md).
- Aby zobaczyć jak eksportować szablon za pośrednictwem interfejsu wiersza polecenia Azure, zobacz [Używanie polecenie Azure dla komputerów Mac, Linux i systemu Windows przy użyciu Menedżera zasobów Azure](xplat-cli-azure-resource-manager.md).
