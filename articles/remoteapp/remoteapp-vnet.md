
<properties
    pageTitle="Sprawdź poprawność VNET Azure do użytku z Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak upewnij się, że VNET usługi Azure jest gotowy do użycia z Azure RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Sprawdź poprawność VNET Azure do użytku z Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Przed użyciem VNET Azure z Azure RemoteApp, można sprawdzić poprawność VNET. Pomaga to zapobiegać problemy z łącznością.

Aby sprawdzić poprawność swojego VNET Azure, wykonaj następujące czynności:

1. Tworzenie Azure maszyn wirtualnych wnętrza podsieci VNET Azure ma być używany z Azure RemoteApp.

2. Nawiązywanie połączenia z tym maszyn wirtualnych za pomocą opcji **Połącz** w portalu zarządzania.
3. Dołączanie maszyny wirtualnej do tej samej domeny, który ma być używany z Azure RemoteApp. Jeśli tworzysz zbioru hybrydowych połączony z siecią lokalnego dołączanie maszyny wirtualnej do domeny lokalnej.

Jeśli to się powiedzie, Azure VNET jest gotowy do użycia z RemoteApp.

Aby uzyskać więcej informacji na temat przepływów pracy zbioru hybrydowych zakończenia do końca zobacz następujące artykuły:

- [Jak planować wirtualnej sieci Azure RemoteApp](remoteapp-planvnet.md)
- [Tworzenie zbioru hybrydowego](remoteapp-create-hybrid-deployment.md)
- [Wdrażanie zbioru Azure RemoteApp usługi Azure wirtualną sieć (z obsługą ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
