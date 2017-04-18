<properties 
   pageTitle="Określanie ustawień DNS w pliku konfiguracji usługi | Microsoft Azure"
   description="Określanie ustawień DNS niestandardowych przy użyciu pliku konfiguracji usługi dla wirtualnej sieci"
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
   ms.date="02/24/2016"
   ms.author="jdial" />

# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Określanie ustawień DNS w pliku konfiguracji usługi

## <a name="dns-elements"></a>Elementy DNS

Plik konfiguracyjny usługi może zawierać element DnsServers z listą adresów IP protokołu IPv4 serwerów systemu nazw domen (DNS), które będzie używane przez usługę. Ustawienia pliku konfiguracji usługi mają pierwszeństwo przed ustawienia pliku konfiguracji sieci. Aby uzyskać więcej informacji zobacz [Schematu konfiguracji usługi Azure (.cscfg pliku)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

>[AZURE.WARNING] Atrybut **nazwy** w elemencie **Serwer_dns** jest używany tylko jako nazwa odwołania. Nie reprezentuje nazwa hosta serwera DNS. Każda wartość atrybutu **Serwer_dns** musi być unikatowa w całej subskrypcji Microsoft Azure.

## <a name="see-also"></a>Zobacz też

[Schemat konfiguracji usługi Azure (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Schemat konfiguracji Azure wirtualnej sieci](http://go.microsoft.com/fwlink/?LinkId=248093)

[Konfigurowanie wirtualnej sieci przy użyciu plików konfiguracji sieci](http://go.microsoft.com/fwlink/?LinkId=248094)

[Informacje o ustawieniach wirtualną sieć w portalu zarządzania](http://go.microsoft.com/fwlink/?LinkId=248092)

