<properties 
   pageTitle="Określanie ustawień DNS w pliku konfiguracji, wirtualną sieć | Microsoft Azure"
   description="Jak zmienić ustawienia serwera DNS w wirtualnej sieci przy użyciu pliku konfiguracji wirtualną sieć w modelu Klasyczny wdrażania"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" 
   tags="azure-service-management" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/23/2016"
   ms.author="jdial" /> 


# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Określanie ustawień DNS w pliku konfiguracji wirtualnej sieci

Plik konfiguracyjny sieci ma dwa elementy, których można określić ustawienia systemu nazw domen (DNS): **DnsServers** i **DnsServerRef**. Możesz dodać listę serwerów DNS, określając ich adresy IP i nazwy odwołanie do elementu **DnsServers** . Aby określić, które wpisy serwera DNS z elementu DnsServers są używane do witryny innej sieci w wirtualnej sieci następnie można użyć elementu **DnsServerRef** .

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia.

Plik konfiguracyjny sieci może zawierać następujące elementy. Tytuł każdego elementu jest połączony z stronę, która zawiera dodatkowe informacje o ustawieniach wartość elementu.

>[AZURE.IMPORTANT] Aby dowiedzieć się, jak skonfigurować plik konfiguracji sieci zobacz [Konfigurowanie wirtualnych sieci przy użyciu pliku konfiguracji sieci](virtual-networks-using-network-configuration-file.md). Aby uzyskać informacji na temat poszczególnych elementów zawartych w pliku konfiguracji sieci zobacz [Azure wirtualna sieć konfiguracji schematu](https://msdn.microsoft.com/library/azure/jj157100.aspx).

[DNS Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

>[AZURE.WARNING] Atrybut **nazwy** w elemencie **Serwer_dns** służy tylko jako odwołanie do elementu **DnsServerRef** . Nie reprezentuje nazwa hosta serwera DNS. Każda wartość atrybutu **Serwer_dns** musi być unikatowa w całej subskrypcji Microsoft Azure

[Element witryny wirtualnej sieci](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

>[AZURE.NOTE] Aby określić to ustawienie dla elementu wirtualnych witryn sieci, go musi być wcześniej zdefiniowany w elemencie DNS. DnsServerRef *nazwy* w elemencie wirtualnych witryn sieci muszą odwoływać się do wartości z pola Nazwa określonego w elemencie DNS Serwer_dns *nazwę*.

## <a name="next-steps"></a>Następne kroki

- Opis [Schemat konfiguracji Azure wirtualnej sieci](http://go.microsoft.com/fwlink/?LinkId=248093).
- Opis [schematu konfiguracji usługi Azure](https://msdn.microsoft.com/library/windowsazure/ee758710).
- [Konfigurowanie sieci wirtualnej korzystanie z plików konfiguracji sieci](virtual-networks-using-network-configuration-file.md).
