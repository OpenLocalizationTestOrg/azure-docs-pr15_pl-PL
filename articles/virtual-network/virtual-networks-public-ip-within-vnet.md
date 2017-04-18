<properties 
   pageTitle="Jak używać publicznych adresów IP w wirtualnej sieci"
   description="Dowiedz się, jak skonfigurować wirtualną sieć, aby użyć publicznych adresów IP"
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
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>Publicznej przestrzeni adresów IP w wirtualnej sieci (VNet)

Wirtualnych sieci (VNets) mogą zawierać publicznych i prywatnych obszarach adresów IP (RFC 1918 adres blokuje). Po dodaniu publicznej zakres adresów IP jest traktowany jako część prywatne przestrzeń adresów VNet IP, która jest tylko dostępne w ramach VNet, połączone VNets i z lokalizacji usługi lokalnego.

Na poniższym obrazie przedstawiono VNet, który zawiera znaki spacji adres IP publicznych i prywatnych.

![Publiczny adres IP koncepcyjny](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>Jak dodać publicznej zakres adresów IP?

Dodawanie publicznej zakres adresów IP taki sam sposób, należy dodać prywatny zakres adresów IP; za pomocą pliku *netcfg* , a także dodając konfiguracji w [Azure portal](http://portal.azure.com). Podczas tworzenia usługi VNet lub możesz przejść wstecz i dodać go później, możesz dodać publicznej zakres adresów IP. W poniższym przykładzie pokazano spacje adresu publicznych i prywatnych adresów IP nie skonfigurowano w tym samym VNet.

![Publiczny adres IP w portalu](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>Czy istnieje ograniczenie?

Istnieje kilka zakresów adresów IP, które nie są dozwolone:

- 224.0.0.0/4 (Multiemisja)

- 255.255.255.255/32 (emisji)

- 127.0.0.0/8 (sprzężenia zwrotnego)

- 169.254.0.0/16 (lokalnego)

- 168.63.129.16/32 (wewnętrzny DNS)

## <a name="next-steps"></a>Następne kroki

[Jak zarządzać serwery DNS używane przez VNet](virtual-networks-manage-dns-in-vnet.md)