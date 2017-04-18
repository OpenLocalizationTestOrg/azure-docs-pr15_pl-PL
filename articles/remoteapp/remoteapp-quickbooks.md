<properties 
    pageTitle="Wdrażanie programu QuickBooks w Azure RemoteApp | Microsoft Azure" 
    description="Dowiedz się, jak udostępniać QuickBooks Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Jak wdrożyć QuickBooks w Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Udostępnianie programu QuickBooks jako aplikację w Azure RemoteApp za pomocą następujące informacje.


Możesz udostępnić QuickBooks 2015 Enterprise Azure RemoteApp w zbiorze hybrydowego lub w chmurze. Plik firmy muszą znajdować się na maszyny uruchomionego programu QuickBooks serwer bazy danych, który jest używany z serwerami Azure RemoteApp. Nigdy nie przechowywać plik firmy na obrazie Azure RemoteApp — utracie danych jest planowane w takim przypadku. Tylko QuickBooks Enterprise obsługuje hostingu pliku programu QuickBooks na udostępnianie zewnętrzne z serwerem bazy danych programu QuickBooks dostępny za pośrednictwem standardowej sieci systemu Windows.   

> [AZURE.IMPORTANT] Serwer bazy danych programu QuickBooks obsługujący plik firmy muszą znajdować się w oddzielnym maszyn wirtualnych w tym samym VNET jako kolekcję Azure RemoteApp.  

## <a name="steps-to-deploy-quickbooks"></a>Kroki wdrażania programu QuickBooks

1. Tworzenie maszyn wirtualnych Azure i instalowanie programu QuickBooks, serwer bazy danych programu QuickBooks, umieść plik firmy na maszyny Azure.  Upewnij się poprawnie skonfigurować reguły zapory.
2. Instalowanie programu QuickBooks na [obraz niestandardowy](remoteapp-imageoptions.md) i utworzyć [zbiór Azure RemoteApp](remoteapp-collections.md), chmury lub hybrydowych w dokładnie VNET tym samym miejsce, w którym znajduje się maszyn wirtualnych, obsługa programu QuickBooks serwer bazy danych za pomocą firmowych plików. 
3.  [Publikowanie](remoteapp-publish.md) Aplikacja programu QuickBooks do użytkowników
4.  Uruchamianie klienta programu QuickBooks hostowanej Azure RemoteApp, przejdź za pomocą standardowej sieci systemu Windows do maszyn wirtualnych, hostingu serwer bazy danych programu QuickBooks i Otwórz plik firmy. 

## <a name="documentation-references"></a>Dokumentacja odwołania

- QuickBooks [obsługiwane konfiguracji](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks [Opcje wdrażania](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Możesz także zajrzeć Ignite prezentacji, [podstawy Microsoft Azure RemoteApp zarządzania i administracji](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) — szybkie przewijanie do przodu do 1:02:45, aby przejść do strony programu QuickBooks.

## <a name="deployment-architecture"></a>Architektura wdrażania

![QuickBooks + wdrażania Azure RemoteApp](./media/remoteapp-quickbooks/ra-quickbooks.png)