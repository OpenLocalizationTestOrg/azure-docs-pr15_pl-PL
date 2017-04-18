<properties
    pageTitle="Stan obsługi w szablonach Menedżera zasobów | Microsoft Azure"
    description="Pokazuje polecane metod dla udostępniać dane o stanie szablony Menedżera zasobów Azure i szablony połączonych za pomocą złożonych obiektów"
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
    ms.date="10/26/2016"
    ms.author="tomfitz"/>

# <a name="sharing-state-in-azure-resource-manager-templates"></a>Udostępnianie stanu w szablonach Azure Menedżera zasobów

W tym temacie przedstawiono najważniejsze wskazówki dotyczące zarządzania i udostępniania Państwa szablonów. Parametry i zmienne wyświetlane w tym temacie przedstawiono przykłady typ obiektów można zdefiniować wygodny sposób organizować wymagań wdrożenia. W tych przykładach można zaimplementować obiekty z wartości właściwości, które miały znaczenie w środowisku usługi.

W tym temacie jest częścią większych dokument. Aby przeczytać pełny papieru, Pobierz [światowej klasy zasobów Menedżer szablonów zagadnienia i sprawdzone rozwiązania](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).


## <a name="provide-standard-configuration-settings"></a>Przekazywać ustawienia konfiguracji standardowej

Zamiast oferuje szablonu, który sumy elastyczność i wiele odmiany, wspólnego wzorca jest zapewnienie zaznaczenia znanych konfiguracji. W praktyce użytkownicy mogą wybrać standardowe rozmiary koszulki, takich jak piaskownicy mały, średni i duży. Inne przykłady rozmiarów koszulki są oferty produktów, takich jak społeczności edition lub enterprise edition. W innych przypadkach może być ustawienia specyficzne dla pracą technologii — takie jak mapy zmniejszyć lub nie sql.

Z obiekty złożone można utworzyć zmiennych, które zawierają zbiorów danych, nazywany "zbiory właściwości" i sterują deklaracji zasobów w szablonie za pomocą tych danych. Rozwiązanie zapewnia dobre, znane konfiguracji o różnych rozmiarach, wstępnie skonfigurowanych dla klientów. Bez znanych konfiguracji użytkowników szablonu należy określić klaster zmiany rozmiaru na własny, współczynnik ograniczeń zasobów platformy i wykonać obliczenia do identyfikowania wyniku podziału konta miejsca do magazynowania i inne zasoby (z powodu ograniczeń zasobów i rozmiar klaster). Oprócz wprowadzania lepiej dla klienta, łatwiej jest kilka znanych konfiguracji pomocy technicznej i ułatwia przedstawianie na wyższy poziom gęstości.

W poniższym przykładzie pokazano sposób definiowania zmiennych zawierających złożone obiekty odpowiadające zbiorów danych. Kolekcje definiowanie wartości, które są używane do rozmiaru maszyn wirtualnych, ustawień sieci, ustawień systemu operacyjnego i ustawienia dostępności.

    "variables": {
      "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
      "tshirtSizeSmall": {
        "vmSize": "Standard_A1",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 1,
          "pool": "db",
          "map": [0,0],
          "jumpbox": 0
        }
      },
      "tshirtSizeMedium": {
        "vmSize": "Standard_A3",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
        "vmCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1],
          "jumpbox": 0
        }
      },
      "tshirtSizeLarge": {
        "vmSize": "Standard_A4",
        "diskSize": 1023,
        "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
        "vmCount": 3,
        "slaveCount": 2,
        "storage": {
          "name": "[parameters('storageAccountNamePrefix')]",
          "count": 2,
          "pool": "db",
          "map": [0,1,1],
          "jumpbox": 0
        }
      },
      "osSettings": {
        "scripts": [
          "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
          "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
        ],
        "imageReference": {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "14.04.2-LTS",
          "version": "latest"
        }
      },
      "networkSettings": {
        "vnetName": "[parameters('virtualNetworkName')]",
        "addressPrefix": "10.0.0.0/16",
        "subnets": {
          "dmz": {
            "name": "dmz",
            "prefix": "10.0.0.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          },
          "data": {
            "name": "data",
            "prefix": "10.0.1.0/24",
            "vnet": "[parameters('virtualNetworkName')]"
          }
        }
      },
      "availabilitySetSettings": {
        "name": "pgsqlAvailabilitySet",
        "fdCount": 3,
        "udCount": 5
      }
    }

Zwróć uwagę, że zmienna **tshirtSize** łączy do tekstu **tshirtSize**Rozmiar koszulki podanych przez parametr (**małe**, **Średnie**, **duże**). Możesz użyć tej zmiennej w celu pobrania zmienna obiektu złożonych skojarzonego dla tego rozmiaru koszulki.

Zmienne w dalszej części tego szablonu można odwołać. Możliwość odwołanie o nazwach zmiennych i ich właściwości upraszcza składnię szablonu i ułatwia opis kontekstu. W poniższym przykładzie zdefiniowano zasobu do wdrożenia przy użyciu obiektów pokazano wcześniej, aby ustawić wartości. Na przykład rozmiar pamięci Wirtualnej jest ustawiona pobierając wartość `variables('tshirtSize').vmSize` podczas wartość dla rozmiar dysku jest pobierana z `variables('tshirtSize').diskSize`. Ponadto identyfikatora URI dla szablonu połączonego jest ustawiona na wartość dla `variables('tshirtSize').vmTemplate`.

    "name": "master-node",
    "type": "Microsoft.Resources/deployments",
    "apiVersion": "2015-01-01",
    "dependsOn": [
        "[concat('Microsoft.Resources/deployments/', 'shared')]"
    ],
    "properties": {
        "mode": "Incremental",
        "templateLink": {
          "uri": "[variables('tshirtSize').vmTemplate]",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "adminPassword": {
            "value": "[parameters('adminPassword')]"
          },
          "replicatorPassword": {
            "value": "[parameters('replicatorPassword')]"
          },
          "osSettings": {
            "value": "[variables('osSettings')]"
          },
          "subnet": {
            "value": "[variables('networkSettings').subnets.data]"
          },
          "commonSettings": {
            "value": {
              "region": "[parameters('region')]",
              "adminUsername": "[parameters('adminUsername')]",
              "namespace": "ms"
            }
          },
          "storageSettings": {
            "value":"[variables('tshirtSize').storage]"
          },
          "machineSettings": {
            "value": {
              "vmSize": "[variables('tshirtSize').vmSize]",
              "diskSize": "[variables('tshirtSize').diskSize]",
              "vmCount": 1,
              "availabilitySet": "[variables('availabilitySetSettings').name]"
            }
          },
          "masterIpAddress": {
            "value": "0"
          },
          "dbType": {
            "value": "MASTER"
          }
        }
      }
    }

## <a name="pass-state-to-a-template"></a>Stan przebiegu do szablonu

Możesz udostępnić stan do szablonu przez parametry, które zapewniają bezpośrednio podczas wdrażania.

W poniższej tabeli przedstawiono najczęściej używanych parametrów w szablonach.

Nazwa | Wartość | Opis
---- | ----- | -----------
Lokalizacja    | Ciąg z ograniczeniami listy regionów Azure   | Lokalizacja miejsce, w którym są rozmieszczane zasobów.
storageAccountNamePrefix    | Ciąg    | DNS unikatową nazwę konta magazynu rozmieszczenie dysków maszyn wirtualnych
nazwa_domeny  | Ciąg    | Nazwa domeny jumpbox publicznie maszyn wirtualnych w formacie: **{nazwa_domeny}. {} Lokalizacja}.cloudapp.com** na przykład: **mydomainname.westus.cloudapp.azure.com**
adminUsername   | Ciąg    | Nazwa użytkownika dla maszyny wirtualne
adminPassword   | Ciąg    | Hasło dla maszyny wirtualne
tshirtSize  | Ciąg z ograniczeniami listy oferowanych rozmiarów koszulki   | Rozmiar jednostki skali nazwanych dostarczaniem. Na przykład "Małe", "Średni", "Duże"
virtualNetworkName  | Ciąg    | Nazwa wirtualną sieć, która konsumenta chce korzystać.
enableJumpbox   | Ciąg z ograniczeniami listy (włączone/wyłączone) | Parametr, który określa, czy włączyć jumpbox dla środowiska. Wartości: "włączone", "wyłączona"

Parametr **tshirtSize** używane w poprzedniej sekcji jest definiowana jako:

    "parameters": {
      "tshirtSize": {
        "type": "string",
        "defaultValue": "Small",
        "allowedValues": [
          "Small",
          "Medium",
          "Large"
        ],
        "metadata": {
          "Description": "T-shirt size of the MongoDB deployment"
        }
      }
    }


## <a name="pass-state-to-linked-templates"></a>Przekazywanie stan do szablonów połączone

Podczas łączenia z szablonów połączone, możesz często Użyj statyczne i wygenerowane zmiennych.

### <a name="static-variables"></a>Zmienne statyczne

Zmienne statyczne są często używane o podanie wartości podstawowej, takich jak adresy URL używane w ramach szablonu.

W poniższym fragmencie szablonu `templateBaseUrl` Określa lokalizację katalogu głównego szablonu w GitHub. Następnego wiersza tworzy nową zmienną `sharedTemplateUrl` który łączy podstawowy adres URL przy użyciu znanej nazwy szablonu zasobów udostępnionych. Pod tym wierszem Zmienna obiektowa złożonych służy do przechowywania Rozmiar koszulki, gdzie podstawowy adres URL jest łączone do lokalizacji szablonów znanej konfiguracji i przechowywane w `vmTemplate` właściwości.

Zaletą tej metody jest, że zmiana lokalizacji szablonu tylko trzeba zmienić zmienną statyczne w jednym miejscu przechodzące go w połączonych szablonów.

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

### <a name="generated-variables"></a>Zmienne wygenerowane

Oprócz zmienne statyczne są generowane dynamicznie wiele zmiennych. Ta sekcja służy do identyfikowania niektóre z często używanych typów wygenerowane zmiennych.

#### <a name="tshirtsize"></a>tshirtSize

Znasz tej zmiennej wygenerowane z powyższych przykładów.

#### <a name="networksettings"></a>networkSettings

Wydajność, funkcja lub szablon rozwiązanie zakresu zakończenia do końca połączonych szablonów przeważnie tworzą zasoby, które występują w sieci. Proste jeden z nich jest użycie złożonych obiektu do przechowywania ustawień sieci i przekazać je do szablonów połączone.

Przykład przekazywania ustawień sieci mogą być widoczne poniżej.

    "networkSettings": {
      "vnetName": "[parameters('virtualNetworkName')]",
      "addressPrefix": "10.0.0.0/16",
      "subnets": {
        "dmz": {
          "name": "dmz",
          "prefix": "10.0.0.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        },
        "data": {
          "name": "data",
          "prefix": "10.0.1.0/24",
          "vnet": "[parameters('virtualNetworkName')]"
        }
      }
    }

#### <a name="availabilitysettings"></a>availabilitySettings

Zasobów utworzone w połączonych szablonów często są umieszczane w zestawie dostępności. W poniższym przykładzie podano nazwę zestawu dostępność, a także błędów domeny i aktualizowanie domeny zliczanie za pomocą.

    "availabilitySetSettings": {
      "name": "pgsqlAvailabilitySet",
      "fdCount": 3,
      "udCount": 5
    }

Jeśli potrzebujesz wiele zestawów dostępności (na przykład, po jednej dla wzorca węzły) i drugi dla węzłów danych, możesz użyć nazwy jako prefiksu, określ wiele zestawów dostępność lub postępuj zgodnie ze wzorem wcześniej związane z tworzeniem zmienną dla określonego rozmiaru koszulki.

#### <a name="storagesettings"></a>storageSettings

Szczegóły miejsca do magazynowania często są udostępnione szablony połączone. W poniższym przykładzie obiekt *storageSettings* zawiera szczegółowe informacje dotyczące nazwy konta i kontener miejsca do magazynowania.

    "storageSettings": {
        "vhdStorageAccountName": "[parameters('storageAccountName')]",
        "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
        "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
    }

#### <a name="ossettings"></a>osSettings

Dzięki szablonom połączone może być konieczne przekazywać ustawienia systemu operacyjnego do różnych typów węzłów różnych typów różnych znanej konfiguracji. Złożone obiekt jest łatwe do przechowywania i udostępniania informacji o systemie operacyjnym i również ułatwia obsługę wielu opcji systemu operacyjnego wdrożenia.

W poniższym przykładzie pokazano obiekt *osSettings*:

    "osSettings": {
      "imageReference": {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "14.04.2-LTS",
        "version": "latest"
      }
    }

#### <a name="machinesettings"></a>machineSettings

Zmienną wygenerowane *machineSettings* jest obiektem złożonych zawierających tekst w wielu zmiennych core związane z tworzeniem maszyny. Zmienne to nazwa użytkownika administratora i hasło, prefiksu dla nazwy maszyn wirtualnych i odwołanie obraz systemu operacyjnego.

    "machineSettings": {
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "machineNamePrefix": "mongodb-",
        "osImageReference": {
            "publisher": "[variables('osFamilySpec').imagePublisher]",
            "offer": "[variables('osFamilySpec').imageOffer]",
            "sku": "[variables('osFamilySpec').imageSKU]",
            "version": "latest"
        }
    },

Należy zauważyć, że ten *osImageReference* pobiera wartości z zmiennej *osSettings* zdefiniowanej w szablonie głównym. Oznacza to, że możesz łatwo zmienić system operacyjny dla maszyny — całkowicie lub na podstawie preferencji konsumenta szablonu.

#### <a name="vmscripts"></a>vmScripts

Obiekt *vmScripts* zawiera szczegóły dotyczące skryptów, aby pobrać i uruchomić wystąpienia maszyn wirtualnych, łącznie z zewnątrz i wewnątrz odwołania. Poza odwołania dotyczą infrastruktury. Odwołania do wewnątrz obejmują programy zainstalowane i konfiguracji.

Właściwość *scriptsToDownload* do wyświetlania listy skryptów, aby pobrać na maszyn wirtualnych. Ten obiekt zawiera również odwołania do argumenty wiersza polecenia dla różnych typów akcji. Te akcje obejmują wykonywanie domyślnej instalacji dla każdego węzła poszczególnych, instalacji uruchamianego po wdrożeniu są wszystkie węzły i wszelkie dodatkowe skrypty, które mogą być specyficzne dla danego szablonu.

W tym przykładzie jest przy użyciu szablonu wdrażane MongoDB, wymagającej kryterium do przeprowadzania wysokiej dostępności. *ArbiterNodeInstallCommand* został dodany do *vmScripts* , aby zainstalować kryterium.

W sekcji Zmienne to, gdzie znaleźć czynników, które definiują określonego tekstu, aby wykonać skrypt z odpowiednie wartości.

    "vmScripts": {
        "scriptsToDownload": [
            "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
            "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
        ],
        "regularNodeInstallCommand": "[variables('installCommand')]",
        "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
        "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
    },


## <a name="return-state-from-a-template"></a>Stan zwrotu przy użyciu szablonu

Nie tylko możesz przekazania danych do szablonu, można także udostępniać danych z powrotem na szablonie połączeń. W sekcji **Wyświetla** połączone szablonu można udostępnić par klucz wartość, jakie mogą zostać użyte w szablonie źródła.

W poniższym przykładzie pokazano, jak przekazać prywatny adres IP wygenerowane w szablonie połączone.

    "outputs": {
        "masterip": {
            "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
            "type": "string"
         }
    }

W głównym szablonu możesz użyć tego danych przy użyciu następującej składni:

    "[reference('master-node').outputs.masterip.value]"

Można użyć tego wyrażenia w sekcji wyjściowe lub sekcji Zasoby głównym szablonu. Wyrażenie nie można używać w sekcji zmienne, ponieważ stan runtime. Aby zwrócić wartość z głównego szablonu, należy użyć:

    "outputs": { 
      "masterIpAddress": {
        "value": "[reference('master-node').outputs.masterip.value]",
        "type": "string"
      }
     
Na przykład za pomocą sekcji wyjściowe szablonu połączonego zwracana dyski danych dla maszyny wirtualnej zobacz [Tworzenie wielu dyskach danych dla maszyny wirtualnej](resource-group-create-multiple.md#creating-multiple-data-disks-for-a-virtual-machine).

## <a name="define-authentication-settings-for-virtual-machine"></a>Definiowanie ustawienia uwierzytelniania dla maszyn wirtualnych

Takim samym wzorcem pokazano wcześniej ustawienia konfiguracji umożliwia określanie ustawień uwierzytelniania dla maszyny wirtualnej. Możesz utworzyć parametru przekazywanie typu uwierzytelniania.

    "parameters": {
      "authenticationType": {
        "allowedValues": [
          "password",
          "sshPublicKey"
        ],
        "defaultValue": "password",
        "metadata": {
          "description": "Authentication type"
        },
        "type": "string"
      }
    }

Dodawanie zmiennych dla typów uwierzytelniania inny, a zmienną do przechowywania typu służy do wdrażania na podstawie wartości parametru.

    "variables": {
      "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
      "osProfilepassword": {
        "adminPassword": "[parameters('adminPassword')]",
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]"
      },
      "osProfilesshPublicKey": {
        "adminUsername": "notused",
        "computerName": "[parameters('vmName')]",
        "customData": "[base64(variables('customData'))]",
        "linuxConfiguration": {
          "disablePasswordAuthentication": "true",
          "ssh": {
            "publicKeys": [
              {
                "keyData": "[parameters('sshPublicKey')]",
                "path": "/home/notused/.ssh/authorized_keys"
              }
            ]
          }
        }
      }
    }

Podczas definiowania maszyny wirtualnej, możesz ustawić **osProfile** zmienną, która została utworzona.

    {
      "type": "Microsoft.Compute/virtualMachines",
      ...
      "osProfile": "[variables('osProfile')]"
    }


## <a name="next-steps"></a>Następne kroki
- Aby uzyskać informacje o części szablonu, zobacz [Tworzenie szablonów Menedżera zasobów Azure](resource-group-authoring-templates.md)
- Aby wyświetlić funkcje, które są dostępne w ramach szablonu, zobacz [Funkcje szablonu Menedżera zasobów Azure](resource-group-template-functions.md)

