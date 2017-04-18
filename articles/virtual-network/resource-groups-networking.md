<properties
   pageTitle="Omówienie dostawcy zasobów sieci | Microsoft Azure"
   description="Dowiedz się więcej o nowych sieci dostawcy zasobów w Menedżerze zasobów Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="network-resource-provider"></a>Dostawca zasobów sieci
Podstawa w bieżącej sukcesu firmy, jest możliwość tworzenia i zarządzania aplikacjami pamiętać o dużych sieci w sposób adaptacyjne, elastyczne, bezpieczne i powtarzalnych. Menedżer zasobów Azure (ARM) umożliwia tworzenie takich aplikacji jako pojedynczy zbiór zasobów w grupach zasobów. Takie zasoby są zarządzane za pomocą różnych dostawców zasobu w obszarze ARM.

Menedżer zasobów Azure zależy od dostawców innego zasobu umożliwia dostęp do zasobów. Istnieją trzy główne źródło dostawców: sieć, przechowywania i obliczeń. W tym dokumencie omówiono cechy i zalety dostawcy zasobów sieci, w tym:

- **Metadane** — możesz dodać informacje do zasobów za pomocą znaczników. Znaczniki może służyć do śledzenia wykorzystania zasobów różnych grup zasobów i subskrypcji.
- **Większą kontrolę nad sieci** — sieci, z którą luźno zasobów i możesz kontrolować je w sposób bardziej szczegółowego. Oznacza to, że masz największą elastyczność zarządzania zasobami sieci.
- **Szybsze konfiguracji** — ponieważ luźno zasobów sieciowych, można utworzyć i dodać akompaniament zasobów sieciowych równolegle. Znaczna zmniejszyła czas konfiguracji.
- **Kontrola dostępu oparta roli** - RBAC zapewnia role domyślne z określonego zakresu zabezpieczeń, oprócz umożliwia utworzenie ról niestandardowych bezpiecznego zarządzania.
- **Łatwiejsze zarządzanie i wdrażanie** — ułatwia wdrażanie i zarządzanie aplikacjami, ponieważ może stos całą aplikację można utworzyć jako pojedynczą kolekcję zasoby w grupie zasobów. I szybszym sposobem wdrożyć, ponieważ można wdrażać, po prostu dostarczając ładunku JSON szablonu.
- **Szybkie dostosowywanie** — korzystanie z szablonów deklaracyjnych stylów, aby włączyć powtarzalnych i szybkie dostosowywanie wdrożeń.
- **Dostosowywanie powtarzalnych** — korzystanie z szablonów deklaracyjnych stylów, aby włączyć powtarzalnych i szybkie dostosowywanie wdrożeń.
- **Interfejsy zarządzania** — umożliwia dowolną z następujących interfejsów zarządzania zasobami:
    - Umieść podstawie interfejsu API
    - Programu PowerShell
    - ZESTAW SDK PROGRAMU .NET
    - Zestaw SDK Node.JS
    - Java SDK
    - Polecenie Azure
    - Preview Portal
    - Język szablonu ARM

## <a name="network-resources"></a>Zasobów
Zasoby sieci mogą teraz zarządzać niezależnie od tego, zamiast ich zarządzane przez zasób pojedynczy obliczeń (maszyna wirtualna). Dzięki temu wyższego poziomu elastyczność i elastyczność w redagowania infrastrukturze złożone i dużych skala w grupie zasobów.

Widok koncepcyjny wdrożenia próbki dotyczące aplikację wielopoziomowymi znajduje się poniżej. Zasoby, które zostanie wyświetlony, takich jak karty sieciowe, publiczne adresy IP i maszyny wirtualne, można zarządzać, niezależnie od siebie.

![Model zasobów sieci](./media/resource-groups-networking/Figure2.png)

Każdy zasób zawiera zestaw właściwości wspólnych i ich poszczególnych właściwości. Wspólne właściwości są:

|Właściwość|Opis|Przykładowe wartości|
|---|---|---|
|**Nazwa**|Nazwa zasobu unikatowe. Każdy typ zasobu ma własny ograniczenia nazewnictwa.|NIC01 PIP01, VM01,|
|**Lokalizacja**|Azure region, w którym znajduje się zasobu|westus, eastus|
|**Identyfikator**|Unikatowy identyfikator URI zależności|/Subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

Możesz sprawdzić poszczególnych właściwości zasobów w poniższych sekcjach.

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## <a name="management-interfaces"></a>Interfejsy zarządzania
Możesz zarządzać Azure zasobów sieci przy użyciu różnych interfejsów. W tym dokumencie firma Microsoft poświęcona tow te interfejsy: interfejsu API usługi REST i szablony.

### <a name="rest-api"></a>INTERFEJSU API USŁUGI REST
Jak wspomniano wcześniej, zasobów mogą być zarządzane przez różne interfejsy, łącznie z interfejsu API usługi REST, .NET SDK, Node.JS SDK, Java SDK, programu PowerShell, polecenie, Azure Portal i szablony.

Interfejsu API usługi Rest odpowiadają specyfikacji protokołu HTTP 1.1. Ogólne struktury identyfikatora URI interfejsu API przedstawiono poniżej:

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

I parametrów w nawiasach klamrowych reprezentują następujące elementy:

- **identyfikator subskrypcji** - identyfikator Azure subskrypcji.
- **obszar nazw dostawcy zasobów** - nazw dla używanego dostawcy. Wartość dla dostawcy zasobów sieci jest *Microsoft.Network*.
- **Nazwa regionu** - nazwa regionu Azure

Podczas tworzenia połączenia do interfejsu API usługi REST są obsługiwane następujące metody HTTP:

- **Umieszczenie** - użyte do utworzenia zasobu danego typu, zmodyfikuj właściwości zasobu lub zmień skojarzenie między zasoby.
- **Uzyskiwanie** - używana do pobierania informacji o ustanawianie zasobu.
- **Usuwanie** — służy do usuwania istniejący zasób.

Żądanie i odpowiedź są zgodne z formatem ładunku JSON. Aby uzyskać więcej informacji zobacz [Azure interfejsów API Menedżera zasobów](https://msdn.microsoft.com/library/azure/dn948464.aspx).

### <a name="arm-template-language"></a>Język szablonu ARM
Oprócz zarządzania zasobami imperatively (za pośrednictwem interfejsów API lub SDK), można również używać deklaracyjnych stylu programowania do tworzenia i zarządzania zasobami sieci przy użyciu języka szablonu ARM.

Poniższe przykładowe reprezentacja szablonu —

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

Szablon jest przede wszystkim opis JSON zasobów i wartości wystąpienia wprowadzone przy użyciu parametrów. W poniższym przykładzie umożliwia tworzenie wirtualnych sieci przy użyciu podsieci 2.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }

          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

Istnieje możliwość ręcznie dostarczania wartości parametrów przy użyciu szablonu lub można użyć pliku parametrów. W poniższym przykładzie pokazano możliwe zestawu wartości parametrów do użycia na podstawie tego szablonu:

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


Główną zaletą korzystania z szablonów są:

- Można tworzyć złożone infrastruktury w grupie zasobów w stylu deklaracyjnych. Aranżacji tworzenia zasoby, w tym zarządzanie zależności jest obsługiwany przez ARM.
- Infrastruktura mogą być tworzone w sposób powtarzalnych wielu różnych regionów i w obszarze po prostu zmienić parametry.
- Styl deklaracyjnych prowadzi do krótszy czas wyprzedzenia w tworzenia szablonów i wdrażania infrastruktury.

Aby uzyskać przykładowe szablony Zobacz [Szablony Azure Szybki Start](https://github.com/Azure/azure-quickstart-templates).

Aby uzyskać więcej informacji na język szablonu ARM zobacz [Język szablonu Azure Menedżera zasobów](../resource-group-authoring-templates.md).

Przykładowy szablon powyżej używa wirtualną sieć i zasoby podsieci. Istnieją inne zasoby sieci, których można używać w sposób opisany poniżej:

### <a name="using-a-template"></a>Przy użyciu szablonu

Można wdrożyć usługi Azure przy użyciu szablonu przy użyciu programu PowerShell, AzureCLI, lub kliknij, aby wdrażanie z GitHub. Aby wdrożyć usługi przy użyciu szablonu w GitHub, wykonaj następujące czynności:

1. Otwórz plik template3 z GitHub. Na przykład otwórz [wirtualnej sieci z dwóch podsieci](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network).
2. Polecenie **Rozmieść Azure**, a następnie zalogować się do portalu Azure z poświadczeniami użytkownika.
3. Sprawdź szablon, a następnie kliknij przycisk **Zapisz**.
4. Kliknij przycisk **Edytuj parametry** i wybierz lokalizację, na przykład *Zachód USA*vnet i podsieci.
5. W razie potrzeby zmień parametry **ADDRESSPREFIX** i **SUBNETPREFIX** , a następnie kliknij **przycisk OK**.
6. Kliknij przycisk **Wybierz grupę zasobów** , a następnie kliknij polecenie Grupa zasobów, aby dodać vnet i podsieci do. Alternatywnie możesz utworzyć nową grupę zasobów, klikając pozycję **lub Utwórz nowe**.
3. Kliknij przycisk **Utwórz**. Zwróć uwagę, kafelków wyświetlanie **inicjowania obsługi administracyjnej szablonu wdrożenia**. Po zakończeniu rozmieszczania zostanie wyświetlony ekran podobny do poniżej.

![Przykładowy szablon wdrożenia](./media/resource-groups-networking/Figure6.png)


## <a name="next-steps"></a>Następne kroki

[Azure język szablonu Menedżera zasobów](../resource-group-authoring-templates.md)

[Azure Networking-często używanych szablonów](https://github.com/Azure/azure-quickstart-templates)

[Obliczanie dostawcy zasobów](../virtual-machines/virtual-machines-windows-compare-deployment-models.md)

[Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)
