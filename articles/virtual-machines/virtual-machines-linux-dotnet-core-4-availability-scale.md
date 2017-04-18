<properties
   pageTitle="Dostępność i skali w Azure szablony Menedżera zasobów | Microsoft Azure"
   description="Samouczek DotNet Core Azure maszyn wirtualnych"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Dostępność i skala w szablonach Azure Menedżera zasobów

Dostępność i skali dotyczą czas pracy i możliwości zapotrzebowania. Jeśli aplikacja musi być 99,9% czasu, jej musi mieć architekturę, która umożliwia dla wielu zasobów jednocześnie obliczeń. Na przykład zamiast jednej witryny sieci Web, konfiguracji z wyższy poziom dostępności zawiera wielu wystąpień tej samej witryny przy użyciu technologii przed ich równoważenia. W tej konfiguracji jedno wystąpienie aplikacji mogą być wykonywane w dół do obsługi, podczas gdy pozostała nadal działać. Skala z drugiej strony odwołuje się do aplikacji możliwość służyć żądanie. Z obciążeniem równomierne zastosowanie, dodawanie lub usuwanie wystąpienia z puli umożliwia aplikacji skalowanie do zapotrzebowania.

Ten dokument, szczegóły konfiguracją wdrożenia przykładowe sklep muzyczny dostępności i Skala. Wszystkie zależności i unikatowe konfiguracje są wyróżnione. Aby uzyskać najlepsze wyniki, wstępnie wdrażać wystąpienie rozwiązanie do subskrypcji Azure i pracy wraz z szablonu Azure Menedżera zasobów. Zakończenie szablonu można znaleźć tutaj — [Wdrożenia sklepu muzyki na Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Konfigurowanie dostępności

Ustawianie dostępności logicznie obejmuje maszyn wirtualnych Azure fizycznie hosts i innych składników infrastrukturalne, takich jak źródła energii i sprzętu sieciowego. Zestawy dostępność upewnij się, że podczas konserwacji, Niepowodzenie urządzeń lub inne przestoje, nie wszystkie maszyn wirtualnych odbywa się. Ustawianie dostępności można dodawać do szablonu Menedżera zasobów Azure przy użyciu programu Visual Studio Kreatora dodawania nowych zasobów lub wstawianie prawidłowych JSON do szablonu.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Ustawianie dostępności](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Ustawianie dostępności jest deklarowane jako właściwość zasobu maszyn wirtualnych. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Ustawianie dostępności skojarzenia z maszyn wirtualnych](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).


```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Dostępność, ustaw widzianej z portalem Azure. Każdy maszyn wirtualnych i szczegółowe informacje o konfiguracji są wyszczególnione w tym miejscu.

![Konfigurowanie dostępności](./media/virtual-machines-linux-dotnet-core/aset.png)

Aby uzyskać szczegółowe informacje na temat zestawów dostępności zobacz [Zarządzanie dostępność maszyn wirtualnych](./virtual-machines-linux-manage-availability.md). 

## <a name="network-load-balancer"></a>Usługi równoważenia obciążenia sieci

Konfigurowanie dostępności przewiduje aplikacji odporność na uszkodzenia, równoważenia obciążenia udostępnia wielu wystąpień aplikacji pod adresem jednej sieci. Wielu wystąpień aplikacji może być przechowywana na wiele maszyn wirtualnych, każdy z nich podłączony do równoważenia obciążenia. Podczas uzyskiwania dostępu do aplikacji równoważenia obciążenia kieruje przychodzące żądanie na elementach dołączony. Usługi równoważenia obciążenia można dodawać przy użyciu programu Visual Studio Kreatora dodawania nowych zasobów lub, wstawiając poprawnie sformatowany JSON zasobu do szablonu Azure Menedżera zasobów.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Równoważenia obciążenia sieci](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

Ponieważ przykładowej aplikacji jest udostępniana w Internecie za pomocą publicznego adresu IP, ten adres jest skojarzony z usługi równoważenia obciążenia. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [skojarzenia równoważenia obciążenia sieci z publiczny adres IP](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Z portalu Azure Omówienie równoważenia obciążenia sieci zawiera skojarzenie z publiczny adres IP.

![Usługi równoważenia obciążenia sieci](./media/virtual-machines-linux-dotnet-core/nlb.png)

## <a name="load-balancer-rule"></a>Reguła równoważenia obciążenia

Podczas korzystania z usługi równoważenia obciążenia skonfigurowanych reguł wpływających na sposób ruch jest strategie między zamierzonego zasobami. Z przykładowej aplikacji sklep muzyczny ruch odebraniu na porcie 80 publiczny adres IP i obejmowała portu 80 maszyn wirtualnych. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Reguły równoważenia obciążenia](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Widok reguły równoważenia obciążenia sieci z portalu.

![Reguła równoważenia obciążenia sieci](./media/virtual-machines-linux-dotnet-core/lbrule.png)

## <a name="load-balancer-probe"></a>Sonda równoważenia obciążenia

Usługi równoważenia obciążenia musi monitora każda maszyna wirtualna tak, aby żądania są obsługiwane tylko do systemami. Monitorowanie odbywa się za stałą wstępnie zdefiniowanych portu. Wdrożenie sklep muzyczny jest skonfigurowany do sondy portu 80 w przypadku wszystkich uwzględniane maszyn wirtualnych. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Sondy równoważenia obciążenia](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

Sonda równoważenia obciążenia zobaczyć Azure portal.

![Sonda usługi równoważenia obciążenia sieci](./media/virtual-machines-linux-dotnet-core/lbprobe.png)

## <a name="inbound-nat-rules"></a>Reguły przychodzące translatora adresów Sieciowych

Podczas korzystania z usługi równoważenia obciążenia, zasady muszą znajdować się w miejsce, w którym dostęp innych niż ładowanie strategii poszczególnych maszyn wirtualnych. Na przykład podczas tworzenia połączenia SSH z każdego maszyn wirtualnych, ruch nie może być równoważenia obciążenia, zamiast ustalonej ścieżkę należy skonfigurować. ścieżki ustalonej jest skonfigurowana przy użyciu zasobu reguły ruchu przychodzącego translatora adresów Sieciowych. Za pomocą tego zasobu, komunikacji przychodzącej można mapować do poszczególnych maszyn wirtualnych. 

W aplikacji sklep muzyczny portu, zaczynając od 5000 są mapowane do portu 22 na każdym komputerze wirtualnych dostępu SSH. `copyindex()` Funkcja służy do zwiększyć przychodzący port tak, aby drugim komputerze wirtualnych otrzymuje przychodzący port 5001, trzecia 5002 i tak dalej.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Reguły przychodzące translatora adresów Sieciowych](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Przykład Reguła ruchu przychodzącego translatora adresów Sieciowych w portalu Azure. Reguły SSH NAT jest tworzony dla każdej maszyny wirtualnej w ramach wdrożenia.

![Reguły przychodzące translatora adresów Sieciowych](./media/virtual-machines-linux-dotnet-core/natrule.png)

Aby uzyskać szczegółowe informacje na temat usługi równoważenia obciążenia sieci Azure zobacz [równoważenia obciążenia dla usług Azure infrastruktury](./virtual-machines-linux-load-balance.md).

## <a name="deploy-multiple-vms"></a>Wdrażanie wielu maszyny wirtualne

Na koniec dla dostępność zestawu lub równoważenia obciążenia skutecznie działać, wiele maszyn wirtualnych są wymagane. Wiele maszyny wirtualne mogą być rozmieszczone za pomocą funkcji kopiowania szablonu Azure Menedżera zasobów. Za pomocą funkcji kopiowania, nie jest konieczne zdefiniować skończonej liczby maszyn wirtualnych, zamiast tej wartości może być dynamicznie udostępniana w czasie wdrażania. Funkcji kopiowania korzysta z liczbę wystąpień utworzone i uchwytami wdrażanie odpowiedniej liczby maszyn wirtualnych i skojarzonych z nimi zasobów.

W szablonie przykładowe sklepu muzyki parametru jest definiowana kierujące liczba wystąpienie. Ten numer jest używany w szablonie podczas tworzenia maszyn wirtualnych i zasoby pokrewne.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
}
```

Na zasób maszyny wirtualnej pętli Kopiuj, wyznaczona nazwę i numer parametru wystąpienia umożliwiają kontrolowanie liczbę kopii wyniku.

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Funkcji kopiowania maszyn wirtualnych](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300). 


```none
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

Bieżący iteracji funkcji kopiowania można uzyskać dostęp z `copyIndex()` funkcji. Wartość funkcji indeks kopii można nadać nazwę maszyn wirtualnych i inne zasoby. Na przykład jeśli dwa wystąpienia maszyny wirtualnej są zainstalowane, muszą różne nazwy. `copyIndex()` Funkcja może służyć jako część nazwy maszyn wirtualnych utworzyć unikatową nazwę. Przykład `copyindex()` funkcja używana do nadawania nazw celów są widoczne w zasób maszyny wirtualnej. W tym miejscu jest to nazwa komputera składa się z `vmName` parametr oraz `copyIndex()` funkcji. 

Wykonaj to łącze, aby wyświetlić przykładowe JSON w szablonie Menedżera zasobów — [Kopiowanie indeks](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319). 


```none
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

`copyIndex` Użyto funkcji kilka razy sklep muzyczny przykładowy szablon. Zasoby i korzystanie z funkcji `copyIndex` zawierać wszystko, które są specyficzne dla jednego wystąpienia maszyny wirtualnej, takie jak interfejs sieciowy, reguły równoważenia obciążenia, a dowolną zależy od funkcji. 

Aby uzyskać więcej informacji dotyczących funkcji kopiowania zobacz [Tworzenie wielu wystąpień zasobów w Menedżerze zasobów Azure](../resource-group-create-multiple.md).

## <a name="next-step"></a>Następny krok

<hr>

[Krok 4 — wdrażaniem aplikacji za pomocą Azure szablony Menedżera zasobów](./virtual-machines-linux-dotnet-core-5-app-deployment.md)