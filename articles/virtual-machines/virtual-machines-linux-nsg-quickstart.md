<properties
   pageTitle="Otwórz porty i protokoły punktów końcowych do maszyny Linux | Microsoft Azure"
   description="Dowiedz się, jak otworzyć port / Tworzenie punktu końcowego do swojego maszyn wirtualnych Linux przy użyciu modelu wdrożenia Menedżera zasobów Azure i polecenie Azure"
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
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Otwieranie portów i punktów końcowych do maszyny Linux platformy Azure
Możesz otworzyć port lub utworzyć punkt końcowy maszyn wirtualnych (maszyn wirtualnych) platformy Azure przez utworzenie filtru sieci w podsieci lub maszyn wirtualnych sieciowej. Tych filtrów, które kontrolować ruchu przychodzącego i wychodzącego, możesz umieścić w grupie zabezpieczeń sieci dołączonych do zasobu pobierającego dane. Używanie grafiki typowy przykład ruchu w sieci web na porcie 80.

## <a name="quick-commands"></a>Szybkie polecenia
Aby utworzyć grupę zabezpieczeń sieci i reguły potrzebujesz [Polecenie Azure](../xplat-cli-install.md) zainstalowany i używania trybu Menedżera zasobów:

```bash
azure config mode arm
```

W poniższych przykładach należy zastąpić przykładowe nazwy parametrów własne wartości. Przykład nazwy parametrów dostępnych `myResourceGroup`, `myNetworkSecurityGroup`, i `myVnet`.

Utworzenie grupy zabezpieczeń sieci, wprowadzając własne nazwy i lokalizacji prawidłowo. Poniższy przykład tworzy grupę zabezpieczeń sieci o nazwie `myNetworkSecurityGroup` w `WestUS` lokalizacji:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Dodaj regułę, aby umożliwić ruchu HTTP na serwerze sieci Web (lub dostosować do swojego własnego scenariusza, takich jak SSH łączności access lub bazy danych). Poniższy przykład tworzy reguły o nazwie `myNetworkSecurityGroupRule` umożliwia ruch TCP na porcie 80:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Grupy zabezpieczeń sieci należy skojarzyć z maszyn wirtualnych sieciowej (NIC). Poniższy przykład kojarzy istniejących NIC o nazwie `myNic` z grupy zabezpieczeń sieci o nazwie `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Można także skojarzyć grupy zabezpieczeń sieci, z wirtualną podsieci, a nie tylko sieciowej na jednym maszyn wirtualnych. Poniższy przykład kojarzy istniejącą podsieć o nazwie `mySubnet` w `myVnet` wirtualnej sieci z grupy zabezpieczeń sieci o nazwie `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Więcej informacji na temat grup zabezpieczeń sieci
Szybkie polecenia tutaj umożliwiają rozpocząć pracę z ruchem ułożony do swojego maszyn wirtualnych. Grupy zabezpieczeń sieci stanowią wielu doskonałe funkcje i dokładności do kontrolowania dostępu do zasobów. Więcej informacji o [tworzeniu reguł ACL tutaj i grupy zabezpieczeń sieci](../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Można zdefiniować grupy zabezpieczeń sieci i reguły ACL jako część szablony Azure Menedżera zasobów. Dowiedz się więcej o [tworzeniu grup zabezpieczeń sieci przy użyciu szablonów](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Jeśli musisz użyć przekierowania portów mapowania unikatowe portu zewnętrznego do wewnętrznego portu usługi maszyn wirtualnych, za pomocą usługi równoważenia obciążenia i reguły tłumaczenia adresów sieciowych (NAT). Na przykład można udostępnić port TCP 8080 zewnętrznie i mieć ruch kierowany do port TCP 80 na maszyny. Aby Dowiedz się więcej o [tworzeniu równoważenia obciążenia w Internecie](../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Następne kroki
W tym przykładzie utworzono proste reguły w celu zezwalanie na ruch HTTP. Możesz znaleźć informacji na temat tworzenia środowiska bardziej szczegółowe w następujących artykułach:

- [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)
- [Co to jest grupa zabezpieczeń sieci (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Omówienie Menedżera zasobów Azure urządzenia do równoważenia obciążenia](../load-balancer2    /load-balancer-arm.md)