
<properties
   pageTitle="Utworzenie pełną środowiska Linux, za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
   description="Tworzenie miejsca do magazynowania, maszyny Linux, wirtualną sieć i podsieci, równoważenia obciążenia, NIC, publiczny adres IP i grupy zabezpieczeń sieci, wszystkie od podstaw za pomocą interfejsu wiersza polecenia Azure."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Tworzenie kompletne środowisko Linux za pomocą interfejsu wiersza polecenia Azure

W tym artykule możemy utworzyć prostą sieć z równoważenia obciążenia i para maszyny wirtualne, które są przydatne w przypadku rozwoju i obliczanie prostych. Przeprowadzimy przez proces polecenie przez polecenie, dopóki nie zostaną dodane dwie pracy, bezpieczne Linux maszyny wirtualne do których można łączyć z dowolnego miejsca w Internecie. Następnie na można przenosić do bardziej złożonych sieci i środowiska.

Wzdłuż sposób możesz informacje o z hierarchią zależności że model wdrożenia Menedżera zasobów umożliwia i znajdują się informacje o ile jej zasilania. Gdy pojawi się, jak wbudowany system, można ponownie utworzyć go szybciej przy użyciu [szablonów Azure Menedżera zasobów](../resource-group-authoring-templates.md). Ponadto po dowiedzieć się, jak części środowiska dopasowane, szablony, aby je zautomatyzować tworzenie staje się łatwiejsze.

Środowisko zawiera:

- Dwa maszyny wirtualne wewnątrz zestawu dostępności.
- Usługi równoważenia obciążenia za pomocą reguły równoważenia obciążenia na porcie 80.
- Sieć zabezpieczeń grupy (NSG) zasady ochrony usługi maszyn wirtualnych niepożądane dane.

![Omówienie podstawowych środowiska](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Aby utworzyć ten niestandardowe środowisko, potrzebujesz najnowszych [Polecenie Azure](../xplat-cli-install.md) w trybie Menedżera zasobów (`azure config mode arm`). Należy również JSON, narzędzia do analizy. W tym przykładzie użyto [jq](https://stedolan.github.io/jq/).

## <a name="quick-commands"></a>Szybkie polecenia
Jeśli chcesz szybko wykonywać zadania, na następujące szczegóły sekcji podstawy polecenia służące do przekazania maszyny Azure. Bardziej szczegółowe informacje i kontekst dla każdego kroku można znaleźć w dalszej części dokumentu, początkowe [tutaj](#detailed-walkthrough).

Upewnij się, że masz [Polecenie Azure](../xplat-cli-install.md) zalogowany i używania trybu Menedżera zasobów:

```bash
azure config mode arm
```

W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Nazwy parametrów przykład obejmują `myResourceGroup`, `mystorageaccount`, i `myVM`.

Tworzenie grupy zasobów. Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `westeurope` lokalizacji:

```bash
azure group create -n myResourceGroup -l westeurope
```

Sprawdź grupa zasobów za pomocą parsera JSON:

```bash
azure group show myResourceGroup --json | jq '.'
```

Utwórz konto miejsca do magazynowania. Poniższy przykład tworzy konto miejsca do magazynowania o nazwie `mystorageaccount` (nazwę konta magazynu musi być unikatowa, aby podać własną unikatową nazwę):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Sprawdź konto miejsca do magazynowania za pomocą parsera JSON:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Tworzenie wirtualnych sieci. Poniższy przykład tworzy wirtualną sieć o nazwie `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Tworzenie podsieci. Poniższy przykład tworzy podsieci o nazwie `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Sprawdź wirtualnej sieci i podsieci za pomocą parsera JSON:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Tworzenie publiczny adres IP. Poniższy przykład tworzy publiczny adres IP, o nazwie `myPublicIP` z nazwą DNS `mypublicdns` (nazwa DNS muszą być unikatowe, tak stanowią własną unikatową nazwę):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Tworzenie usługi równoważenia obciążenia. Poniższy przykład tworzy usługi równoważenia obciążenia o nazwie `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Tworzenie puli zewnętrzną adresów IP dla usługi równoważenia obciążenia i kojarzenie publiczny adres IP. Poniższy przykład tworzy zewnętrzną puli IP o nazwie `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Tworzenie puli wewnętrznej adresów IP dla usługi równoważenia obciążenia. Poniższy przykład tworzy puli adresów IP wewnętrznej o nazwie `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Tworzenie sieci przychodzącego SSH reguł translatora adresów adres usługi równoważenia obciążenia. Poniższy przykład tworzy dwie reguły równoważenia obciążenia, `myLoadBalancerRuleSSH1` i `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Utwórz sieć web reguł translatora adresów Sieciowych do równoważenia obciążenia ruchu przychodzącego. Poniższy przykład tworzy reguły równoważenia obciążenia o nazwie`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Tworzenie sondy kondycji usługi równoważenia obciążenia. Poniższy przykład tworzy sondy TCP o nazwie `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Sprawdź równoważenia obciążenia, pule IP i reguły translatora adresów Sieciowych za pomocą parsera JSON:

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Tworzenie pierwszej karty sieciowej (NIC). Zamienianie `#####-###-###` sekcji za pomocą własnego identyfikatora Azure subskrypcji. Twoja subskrypcja identyfikator zostanie zapisana w danych wyjściowych `jq` podczas sprawdzania tworzysz zasobów. Możesz także wyświetlić identyfikator subskrypcji z `azure account list`. 

Poniższy przykład tworzy NIC o nazwie `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Tworzenie drugiej karty sieciowej. Poniższy przykład tworzy NIC o nazwie `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Sprawdź dwóch nic za pomocą parsera JSON:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Tworzenie grupy zabezpieczeń sieci. Poniższy przykład tworzy grupę zabezpieczeń sieci o nazwie `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Dodawanie dwóch reguły przychodzące dla grupy zabezpieczeń sieci. Poniższy przykład tworzy dwie reguły, `myNetworkSecurityGroupRuleSSH` i `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Sprawdź grupy zabezpieczeń sieci i reguły przychodzące za pomocą parsera JSON:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Grupa zabezpieczeń sieci powiązania z dwóch sieciowe:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Tworzenie zestawu dostępności. Poniższy przykład tworzy dostępność nazwanego zestawu `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Tworzenie pierwszego maszyn wirtualnych Linux. Poniższy przykład tworzy maszyny o nazwie `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Utwórz drugą maszyn wirtualnych Linux. Poniższy przykład tworzy maszyny o nazwie `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Za pomocą parsera JSON Sprawdź, czy wszystkie elementy, który został utworzony:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Nowe środowisko wyeksportować do szablonu w celu ponownego szybko tworzyć nowe wystąpienia:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Uzyskać szczegółowy opis
Szczegółowe opisy czynności poniżej wyjaśniono, każdego polecenia czynności podczas tworzenia się środowiska. Poniższe pojęcia są pomocne podczas tworzenia własnych niestandardowych środowiska rozwoju lub produkcji.

Upewnij się, że masz [Polecenie Azure](../xplat-cli-install.md) zalogowany i używania trybu Menedżera zasobów:

```bash
azure config mode arm
```

W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Nazwy parametrów przykład obejmują `myResourceGroup`, `mystorageaccount`, i `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Tworzenie grup zasobów i wybierz pozycję lokalizacje wdrażania

Grupy zasobów Azure to jednostki wdrażania, które zawierają informacje o konfiguracji i metadanych służących do zarządzania logiczne wdrożeń zasobów. Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `westeurope` lokalizacji:

```bash
azure group create --name myResourceGroup --location westeurope
```

Wynik:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

Potrzebujesz konta miejsca do magazynowania dla dysków maszyn wirtualnych i dla wszystkich dysków dodatkowe dane, które chcesz dodać. Możesz utworzyć konta miejsca do magazynowania niemal natychmiast po utworzeniu grupy zasobów.

W tym miejscu firma Microsoft korzysta z `azure storage account create` polecenia przechodzące konta, grupa zasobów, który określa, a typ obsługi miejsca do magazynowania ma. Poniższy przykład tworzy konto miejsca do magazynowania o nazwie `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Wynik:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Sprawdzenie naszych grupa zasobów za pomocą `azure group show` polecenia, można użyć narzędzia [jq](https://stedolan.github.io/jq/) wraz z `--json` opcję polecenie Azure. (Służy **jsawk** lub dowolnej bibliotece język, aby przeanalizować JSON.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Wynik:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Aby zbadać konta miejsca do magazynowania przy użyciu interfejsu wiersza polecenia, najpierw należy ustawić nazwy kont i klawiszy. Zastąp nazwę, którą należy wybrać nazwę konta magazynu w poniższym przykładzie:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Następnie można łatwo wyświetlanie informacji o miejsca do magazynowania:

```bash
azure storage container list
```

Wynik:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Tworzenie wirtualnych sieci i podsieci

Zamierzasz dalej do utworzenia wirtualną sieć działa Azure i podsieci, w której można utworzyć pośrednictwem usługi SMS. Poniższy przykład tworzy wirtualną sieć o nazwie `myVnet` z `192.168.0.0/16` prefiksu adresu:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Wynik:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Ponownie, można użyć opcji — json `azure group show` i **jq** , aby zobaczyć, jak w przypadku tworzenia naszych zasobów. Teraz `storageAccounts` zasobów i `virtualNetworks` zasobów.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Wynik:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Teraz Tworzenie podsieci `myVnet` wirtualnej sieci, w której są wdrożone maszyny wirtualne. Firma Microsoft korzysta z `azure network vnet subnet create` polecenia wraz z zasobami mamy już utworzony: `myResourceGroup` grupa zasobów i `myVnet` wirtualnej sieci. W poniższym przykładzie dodajemy podsieci o nazwie `mySubnet` z prefiksem adresu podsieci `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Wynik:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Ponieważ podsieci logicznie znajduje się wewnątrz wirtualnej sieci, możemy poszukaj informacji podsieci za pomocą polecenia nieco inne. Polecenie Firma Microsoft korzysta z jest `azure network vnet show`, ale w dalszym ciągu Przeanalizuj wyjście JSON przy użyciu **jq**.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Wynik:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Tworzenie publiczny adres IP (PIP)

Teraz tworzenie publiczny adres IP (PIP), który możemy przypisać do usługi równoważenia obciążenia. Umożliwia połączyć się z maszyny wirtualne z Internetu, przy użyciu `azure network public-ip create` polecenia. Ponieważ domyślny adres jest dynamiczne, możemy utworzyć w domenie **cloudapp.azure.com** nazwanego wpisu DNS za pomocą `--domain-name-label` opcji. Poniższy przykład tworzy publiczny adres IP, o nazwie `myPublicIP` z nazwą DNS `mypublicdns`. Jak nazwa DNS musi być unikatowa, wprowadź własną unikatową nazwę DNS:

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Wynik:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Publiczny adres IP jest również zasób najwyższego poziomu, dzięki której będziesz widzieć za pomocą `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Wynik:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Możesz przejrzeć więcej szczegółów zasobów, w tym w pełni kwalifikowaną nazwę domeny (FQDN) programu poddomeny, korzystając z pełną `azure network public-ip show` polecenia. Zasób publiczny adres IP został przydzielony logicznie, ale nie został jeszcze przypisany określony adres. Aby uzyskać adres IP, możesz zacząć potrzebujesz równoważenia obciążenia, który nie został jeszcze utworzony.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Wynik:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Tworzenie równoważenia obciążenia i pule adresów IP
Po utworzeniu równoważenia obciążenia umożliwia rozłożenie ruchu w wielu maszyny wirtualne. Umożliwia także nadmiarowości w aplikacji, uruchamianie wielu maszyn wirtualnych, które odpowiadają na żądania użytkownika w przypadku konserwacji lub dużym obciążeniem. Poniższy przykład tworzy usługi równoważenia obciążenia o nazwie `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Wynik:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Nasze równoważenia obciążenia jest dość pusty, dlatego tworzenie pul niektórych adresów IP. Chcemy utworzyć dwa pule adresów IP dla naszych równoważenia obciążenia — jedną dla zewnętrznej i jedną dla wewnętrzna. Zewnętrzną pula adresów IP jest widoczna publicznie. Należy również lokalizacji, do której możemy przypisywanie PIP, który wcześniej utworzone. Następnie używamy puli wewnętrznej jako lokalizacji dla naszych maszyny wirtualne Aby nawiązać połączenie. W ten sposób dane może przechodzić za pośrednictwem usługi równoważenia obciążenia do maszyny wirtualne.

Najpierw tworzenie naszych zewnętrzną puli adresów IP. Poniższy przykład tworzy zewnętrzną puli nazwane `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Wynik:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Należy zauważyć, jak użyliśmy `--public-ip-name` Przełącz przenieść `myPublicIP` możemy utworzone wcześniej przez. Przypisywanie publiczny adres IP do równoważenia obciążenia umożliwia osiągnięcia pośrednictwem usługi SMS w Internecie.

Następnie tworzenie naszych drugiego puli IP, tym razem naszych ruchu wewnętrznej. Poniższy przykład tworzy puli wewnętrznej o nazwie `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Wynik:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Widać, jak robi naszych równoważenia obciążenia, wyświetlając z `azure network lb show` i sprawdzania wyjściowe JSON:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Wynik:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Tworzenie równoważenia obciążenia reguł translatora adresów Sieciowych
Aby uzyskać ruch ułożony przez naszych równoważenia obciążenia, ma zostać utworzony adres sieciowy reguły translatora adresów, określające akcje przychodzących i wychodzących. Możesz określić protokół ma być używany, a następnie zamapuj zewnętrzne porty portów wewnętrznych w razie potrzeby. W naszym środowisku tworzenie niektóre reguły, które zezwalają SSH przez naszych równoważenia obciążenia do naszych maszyny wirtualne. Firma Microsoft skonfigurować porty TCP 4222 i 4223 bezpośrednio do TCP port 22 w naszym maszyny wirtualne (które tworzymy później). Poniższy przykład tworzy regułę o nazwie `myLoadBalancerRuleSSH1` do zamapować TCP port 4222 do portu 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Wynik:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Powtórz tę procedurę dla drugiej reguły translatora adresów Sieciowych dla SSH. Poniższy przykład tworzy regułę o nazwie `myLoadBalancerRuleSSH2` do zamapować TCP port 4223 do portu 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Załóżmy też wtyczce i utworzyć regułę NAT port TCP 80 dla ruchu w sieci web, podczas regułę do naszych pul adresów IP. Jeśli firma Microsoft Podłączanie reguły do puli adresów IP, zamiast podczas regułę, aby nasze maszyny wirtualne pojedynczo, możemy Dodawanie lub usuwanie maszyny wirtualne z puli adresów IP. Usługi równoważenia obciążenia jest automatycznie dostosowywany tak przepływu ruchu. Poniższy przykład tworzy regułę o nazwie `myLoadBalancerRuleWeb` do zamapować TCP port 80 do portu 80:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Wynik:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Tworzenie sondy kondycji usługi równoważenia obciążenia

Kondycja okresowo sondy kontroli maszyny wirtualne znajdujących się za naszych równoważenia obciążenia, aby upewnić się, ich działania i odpowiadać na żądania, zgodnie z definicją. W przeciwnym razie usuwane z operacji, aby upewnić się, że użytkownicy nie są kierowane do ich. Można zdefiniować niestandardowe sprawdza sondy kondycji, wraz z interwały i wartości limitu czasu. Aby uzyskać więcej informacji o kondycji sondy zobacz [sondy równoważenia obciążenia](../load-balancer/load-balancer-custom-probe-overview.md). Poniższy przykład tworzy TCP kondycji sondowany nazwany `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Wynik:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Tutaj możemy określony interwał 15 sekundach dla naszych sprawdzanie kondycji. Firma Microsoft może przeoczyć maksymalnie czterech sondy (jednej minuty), zanim równoważenia obciążenia uważa, że host już nie działa.

## <a name="verify-the-load-balancer"></a>Sprawdź równoważenia obciążenia
Po zakończeniu konfiguracji równoważenia obciążenia. Poniżej przedstawiono kroki wykonane:

1. Najpierw musisz utworzyć równoważenia obciążenia.
2. Następnie utworzone zewnętrzną puli adresów IP i do niego przypisana publiczny adres IP.
3. Następnie utworzono wewnętrznej puli adresów IP, którego maszyny wirtualne można nawiązać połączenie.
4. Po wykonaniu tej utworzonej reguły translatora adresów Sieciowych, które zezwalają SSH do maszyny wirtualne zarządzania, wraz z regułę, która zezwala port TCP 80 dla naszych aplikacji sieci web.
5. Na koniec po dodaniu sondy kondycji do okresowego sprawdzania maszyny wirtualne. Sonda kondycji zapewnia użytkownicy nie próbować uzyskać dostęp do maszyn wirtualnych, który jest już nie działa lub serwowania zawartości.

Przeanalizujmy usługi równoważenia obciążenia wygląda teraz:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Wynik:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Tworzenie NIC do użytku z maszyn wirtualnych Linux

Karty sieciowe są programowo dostępne, ponieważ można stosować reguły do użytku. Można także używać więcej niż jeden. Poniższa `azure network nic create` polecenia nawiązać połączenie NIC puli adresów IP wewnętrznej obciążenia i skojarzyć regułę translatora adresów Sieciowych, aby zezwolić na ruch SSH.
 
Zamienianie `#####-###-###` sekcji za pomocą własnego identyfikatora Azure subskrypcji. Twoja subskrypcja identyfikator zostanie zapisana w danych wyjściowych `jq` podczas sprawdzania tworzysz zasobów. Możesz także wyświetlić identyfikator subskrypcji z `azure account list`. 

Poniższy przykład tworzy NIC o nazwie `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Wynik:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Aby wyświetlić szczegóły, należy bezpośrednio zbadaniu tego zasobu. Zbadać zasobu, używając `azure network nic show` polecenie:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Wynik:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Teraz możemy utworzyć druga karta NIC podczas w naszym wewnętrznej puli adresów IP ponownie. Ta reguła translatora adresów Sieciowych czasu drugi umożliwia ruch SSH. Poniższy przykład tworzy NIC o nazwie `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Tworzenie grupy zabezpieczeń sieci i reguł

Teraz możemy utworzyć grupę zabezpieczeń sieci i ruchu przychodzącego zasady dostępu do karty sieciowej. Grupy zabezpieczeń sieci można stosować do karty Sieciowej lub podsieci. Definiowanie reguły, aby kontrolować przepływ ruchu i pośrednictwem usługi SMS. Poniższy przykład tworzy grupę zabezpieczeń sieci o nazwie `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Możesz dodać Reguła ruchu przychodzącego dla NSG do zezwalania na połączenia przychodzącego na porcie 22 (do obsługi SSH). Poniższy przykład tworzy regułę o nazwie `myNetworkSecurityGroupRuleSSH` umożliwia TCP port 22:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Teraz Dodawanie Reguła ruchu przychodzącego dla NSG do zezwalania na połączenia przychodzącego na porcie 80 (do obsługi ruchu w sieci web). Poniższy przykład tworzy regułę o nazwie `myNetworkSecurityGroupRuleHTTP` umożliwia TCP port 80:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] Reguła ruchu przychodzącego jest filtr dla połączeń przychodzących. W tym przykładzie możemy powiązać NSG NIC wirtualnej maszyny wirtualne, co oznacza, że każdego wezwania do portu 22 jest przekazywany za pośrednictwem do karty Sieciowej w naszym maszyn wirtualnych. Ta reguła przychodzących jest o połączenie sieciowe, a nie o punktu końcowego, która jest co będzie o we wdrożeniach klasyczny. Aby otworzyć port, należy pozostawić `--source-port-range` równa "\*" (wartość domyślna) do akceptowania przychodzących żądań z **dowolnego** żądania portu. Porty są zwykle dynamiczne.

## <a name="bind-to-the-nic"></a>Należy powiązać NIC

Należy powiązać NSG sieciowe. Trzeba połączyć naszych nic z naszych grupy zabezpieczeń sieci. Uruchomienie obu poleceń, aby nawiązać połączenie jedną z naszych nic:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Tworzenie zestawu dostępności
Dostępność ustawia widoku Pomocy usługi maszyny wirtualne wielu domen błędów i uaktualniania domen. Tworzenie dostępności ustawieniem pośrednictwem usługi SMS. Poniższy przykład tworzy dostępność nazwanego zestawu `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Błąd domen Definiowanie grup maszyn wirtualnych, które współużytkują typowe przełącznika źródła i sieci power. Domyślnie maszyn wirtualnych, które są skonfigurowane w obrębie zestawu dostępności są oddzielone domen maksymalnie trzy błędów. Są to problem sprzętu w jednej z tych domen błędów nie wpływa na każdej maszyn wirtualnych uruchomionym aplikacji. Azure automatycznie rozdziela maszyny wirtualne domen błędów podczas umieszczania ich w zestawie dostępności.

Uaktualnianie domen wskazują grupy maszyn wirtualnych i źródłowych sprzętu, który należy ponownie uruchomić, w tym samym czasie. Kolejność, w której są ponownie uruchomić uaktualnianie domen może nie być sekwencyjne podczas planowanej konserwacji, ale tylko jedno uaktualnienie uruchomieniu naraz. Ponownie Azure automatycznie rozdziela pośrednictwem usługi SMS domen uaktualnienia podczas umieszczania ich w witrynie dostępności.

Dowiedz się więcej na temat [zarządzania dostępność maszyny wirtualne](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>Tworzenie maszyny wirtualne Linux

Została utworzona zasobów sieci i miejsca do magazynowania do pomocy technicznej dostępne Internet maszyny wirtualne. Teraz Przyjrzyjmy tworzyć takich za pośrednictwem SMS i zabezpieczanie ich kluczem SSH, która nie zawiera hasła. W tym przypadku możemy zacząć tworzenie Ubuntu maszyn wirtualnych według ostatnich KÓW. Firma Microsoft zlokalizować te informacje obrazu przy użyciu `azure vm image list`, zgodnie z opisem w [znajdowaniu obrazów maszyn wirtualnych Azure](virtual-machines-linux-cli-ps-findimage.md).

Wybraliśmy obrazu przy użyciu polecenia `azure vm image list westeurope canonical | grep LTS`. W tym przypadku firma Microsoft korzysta z `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Dla ostatniego pola, możemy przekazać `latest` tak, aby w przyszłości będziemy zawsze korzystać z najnowszej kompilacji. (Ciąg, firma Microsoft korzysta z `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

Ten następnym krokiem jest znany dla każdego, kto utworzył już ssh kluczy publicznych i prywatnych rsa parowanie na Linux lub Mac przy użyciu **ssh-keygen-t rsa -b 2048**. Jeśli nie masz dowolnego pary kluczy certyfikatu swojej `~/.ssh` katalogów, można je utworzyć:

- Automatycznie, za pomocą `azure vm create --generate-ssh-keys` opcji.
- Ręcznie korzystając [z instrukcjami, aby je utworzyć samodzielnie](virtual-machines-linux-mac-create-ssh-keys.md).

Opcjonalnie możesz użyć `--admin-password` metody uwierzytelniania połączenia SSH po utworzeniu maszyn wirtualnych. Ta metoda jest zazwyczaj mniej bezpieczne.

Tworzymy maszyn wirtualnych przez dostosowanie wszystkich naszych zasobów i informacji wraz z `azure vm create` polecenie:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Wynik:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Można nawiązać z maszyn wirtualnych bezpośrednio przy użyciu kluczy SSH domyślne. Upewnij się, określ odpowiedni port, ponieważ możemy przechodząc za pośrednictwem usługi równoważenia obciążenia. (Dla naszych pierwszego Głosowa, możemy skonfigurować reguły translatora adresów Sieciowych przekazywania port 4222 do naszych maszyn wirtualnych):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Wynik:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Zrealizuj i Utwórz do drugiego maszyn wirtualnych w taki sam sposób:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

I za pomocą można teraz `azure vm show myResourceGroup myVM1` polecenie, aby sprawdzić, jakie został utworzony. W tym momencie korzystasz z usługi maszyny wirtualne Ubuntu za równoważenia obciążenia platformy Azure, który możesz zalogować się do tylko w przypadku usługi pary kluczy SSH (ponieważ hasła są wyłączone). Możesz zainstalować nginx lub host z wieloma adresami, wdrażanie aplikacji sieci web i wyświetlić dane przepływu za pośrednictwem usługi równoważenia obciążenia do obu maszyny wirtualne.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Wynik:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Eksportowanie środowiska jako szablonu
Teraz, gdy skonstruowaniu to środowisko, co zrobić, jeśli chcesz utworzyć środowisko projektowania dodatkowe z tymi samymi parametrami lub środowisku produkcyjnym, która jest zgodna z jego? Menedżer zasobów przy użyciu szablonów JSON, które definiują wszystkie parametry w środowisku usługi. Tworzenie się całego środowiska przez odwoływanie się do tego szablonu JSON. Można [tworzyć szablony JSON ręcznie](../resource-group-authoring-templates.md) lub eksportowanie istniejące środowisko do utworzenia szablonu JSON:

```bash
azure group export --name myResourceGroup
```

To polecenie tworzy `myResourceGroup.json` plik w bieżącym katalogu roboczym. Podczas tworzenia środowiska usługi przy użyciu tego szablonu, zostanie wyświetlony monit o wszystkich nazw zasobów, łącznie z nazwami równoważenia obciążenia, interfejsy lub maszyny wirtualne. Można wypełniać tych nazw w pliku szablonu, dodając `-p` lub `--includeParameterDefaultValue` parametr `azure group export` polecenie widoczną wcześniej. Edytowanie szablonu JSON, aby określić nazwy zasobów lub [utworzyć plik parameters.json](../resource-group-authoring-templates.md#parameters) określająca nazw zasobów.

Aby utworzyć środowisko z szablonu:

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Można przeczytać [więcej informacji na temat sposobu wdrażania przy użyciu szablonów](../resource-group-template-deploy-cli.md). Informacje na temat sposobu stopniowo aktualizacji środowiskach, za pomocą pliku parametrów i uzyskać dostęp do szablonów z lokalizacji przechowywania jednym.

## <a name="next-steps"></a>Następne kroki

Teraz możesz rozpocząć pracę z wielu składników sieci i maszyny wirtualne. To środowisko przykładowych służy do tworzenia aplikacji przy użyciu podstawowe składniki wprowadzone w tym miejscu.
