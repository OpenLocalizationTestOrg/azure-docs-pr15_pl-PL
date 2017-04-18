<properties
   pageTitle="Tworzenie maszyny Linux przy użyciu kart | Microsoft Azure"
   description="Dowiedz się, jak utworzyć maszyny Linux przy użyciu kart dołączone do niej przy użyciu szablonów polecenie Azure lub Menedżer zasobów."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Tworzenie maszyny Linux za pomocą kart
Możesz utworzyć maszyny wirtualnej (maszyn wirtualnych) platformy Azure zawierającej wiele interfejsów wirtualnych sieci (NIC) dołączone do niego. Typowy scenariusz będzie mieć różnych podsieci łączności frontonu i wewnętrznej lub sieci przeznaczone do rozwiązania monitorowania lub kopii zapasowej. Ten artykuł zawiera szybkie polecenia do tworzenia maszyn wirtualnych z kart dołączone do niego. Aby uzyskać szczegółowe informacje, tym ich tworzenie kart w ramach własnych skryptów imprezie więcej informacji o [wdrażaniu maszyny wirtualne NIC wielokrotne](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Różne [rozmiary maszyn wirtualnych](virtual-machines-linux-sizes.md) obsługi różną liczbę kart sieciowych, więc odpowiednio rozmiar do maszyn wirtualnych.

>[AZURE.WARNING] Po utworzeniu maszyny - nic nie można dodać do istniejącego maszyn wirtualnych, należy dołączyć kart. Możesz [utworzyć maszyny oparte na oryginalnym dyskach wirtualnych](virtual-machines-linux-copy-vm.md) i tworzyć kart wdrażanie maszyn wirtualnych.

## <a name="quick-commands"></a>Szybkie polecenia
Upewnij się, że masz [Polecenie Azure](../xplat-cli-install.md) zalogowany i używania trybu Menedżera zasobów:

```bash
azure config mode arm
```

W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Przykład nazwy parametrów dostępnych `myResourceGroup`, `mystorageaccount`, i `myVM`.

Najpierw należy utworzyć grupy zasobów. Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `WestUS` lokalizacji:

```bash
azure group create myResourceGroup -l WestUS
```

Utwórz konto miejsca do magazynowania do przechowywania pośrednictwem usługi SMS. Poniższy przykład tworzy konto miejsca do magazynowania o nazwie `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Tworzenie wirtualnej sieci nawiązania połączenia z pośrednictwem SMS do. Poniższy przykład tworzy wirtualną sieć o nazwie `myVnet` z prefiksu adresu `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Tworzenie dwóch podsieci wirtualną sieć — jedną dla zewnętrznej ruchu i jedną dla ruchu wewnętrznej. W poniższym przykładzie tworzone dwa podsieci, o nazwie `mySubnetFrontEnd` i `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Utwórz dwa nic, dołączania jednego NIC do zewnętrznej podsieci i jedna karta NIC do podsieci wewnętrznej. W poniższym przykładzie tworzone dwa nic, o nazwie `myNic1` i `myNic2`i dołącza je do swojego podsieci:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Na koniec Tworzenie maszyn wirtualnych, dołączanie dwóch sieciowe zostało wcześniej utworzone. Poniższy przykład tworzy maszyny o nazwie `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Tworzenie kart za pomocą interfejsu wiersza polecenia Azure
Jeśli wcześniej utworzono maszyny za pomocą interfejsu wiersza polecenia Azure, szybkie polecenia wymaga znajomości. Ten proces jest taki sam, aby utworzyć jeden NIC lub kart. Można znaleźć więcej informacji na temat [wdrażania kart za pomocą interfejsu wiersza polecenia Azure](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), tym skrypty proces w pętli do tworzenia wszystkich sieciowe.

W poniższym przykładzie tworzone dwa nic, o nazwie `myNic1` i `myNic2`, z jednego NIC nawiązywanie połączenia z każdą:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Zwykle można także tworzyć [Grupy zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md) lub [Usługa równoważenia obciążenia](../load-balancer/load-balancer-overview.md) dzięki rozłożenie ruch w Twojej maszyny wirtualne i zarządzanie nimi. Ponownie polecenia są takie same podczas pracy z kart. Poniższy przykład tworzy grupę zabezpieczeń sieci o nazwie `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Powiązać przy użyciu grupy zabezpieczeń sieci usługi sieciowe `azure network nic set`. Poniższy przykład powiąże `myNic1` i `myNic2` z `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Podczas tworzenia maszyn wirtualnych, możesz teraz podać kart. Wolisz przy użyciu `--nic-name` aby zapewnić pojedynczy NIC, użyj zamiast tego możesz `--nic-names` i podaj rozdzielaną średnikami listę kart sieciowych. Należy również zwrócić uwagę po wybraniu rozmiar pamięci Wirtualnej. Istnieją limity dotyczące całkowitą liczbę kart sieciowych, które można dodać do maszyny. Dowiedz się więcej na temat [rozmiarów maszyn wirtualnych Linux](virtual-machines-linux-sizes.md). W poniższym przykładzie pokazano sposób określania kart, a następnie rozmiar pamięci Wirtualnej, która obsługuje przy użyciu kart (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Tworzenie kart przy użyciu szablonów Menedżera zasobów
Azure szablony Menedżera zasobów umożliwia definiowanie środowiska deklaracyjnych pliki JSON. [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)można czytać. Szablony Menedżera zasobów umożliwia tworzenie wielu wystąpień zasobu podczas wdrażania, takich jak tworzenie kart. Określ liczbę wystąpień, aby utworzyć za pomocą *kopiowania* :

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Dowiedz się więcej o [tworzeniu wielu wystąpień za pomocą *kopiowania*](../resource-group-create-multiple.md). 

Można również użyć `copyIndex()` następnie dołączyć numeru do nazwy zasobów, która umożliwia tworzenie `myNic1`, `myNic2`itp. Poniżej przedstawiono przykład dołączanie wartość indeksu:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Można przeczytać pełny przykład [tworzenia kart przy użyciu szablonów Menedżera zasobów](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Następne kroki
Upewnij się, przejrzyj [rozmiarów Linux maszyn wirtualnych](virtual-machines-linux-sizes.md) podczas próby Tworzenie maszyn wirtualnych za pomocą kart. Należy zwrócić uwagę na maksymalna liczba nic obsługuje każdego rozmiar pamięci Wirtualnej. 

Należy pamiętać, że nie możesz dodać dodatkowe nic do istniejącego maszyn wirtualnych, należy utworzyć wszystkie sieciowe podczas wdrażania maszyn wirtualnych. Zwrócić uwagę podczas planowania wdrożeń aby upewnić się, że wszystkie łączność sieciowa wymagane od początku.