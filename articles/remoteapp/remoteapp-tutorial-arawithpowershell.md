<properties
   pageTitle="Używanie poleceń cmdlet programu PowerShell z Azure RemoteApp | Microsoft Azure"
   description="Dowiedz się, jak używać polecenia cmdlet programu Windows PowerShell w Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Polecenia cmdlet programu Windows PowerShell za pomocą Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

 Polecenia cmdlet programu RemoteApp PowerShell Azure umożliwia administrowanie i obsługa kolekcji. Aby rozpocząć pracę, korzystając z następujących informacji.

## <a name="get-the-cmdlets"></a>Pobieranie i poleceń cmdlet 
-------------
Najpierw pobierz polecenia cmdlet programu Powershell Azure [tutaj](http://go.microsoft.com/?linkid=9811175)RemoteApp poleceń cmdlet znajdują się w nim. 

Zapoznaj się z [polecenia cmdlet Azure RemoteApp pomocy](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Konfigurowanie Azure polecenia cmdlet, aby korzystać z subskrypcji
------------------
Wykonaj [tego przewodnika](../powershell-install-configure.md) , dzięki czemu można użyć poleceń cmdlet przed subskrypcji Azure.

Aby szybko rozpocząć można wykonaj następujące kroki:

1.  Pobierz i zainstaluj [Azure poleceń cmdlet](http://go.microsoft.com/?linkid=9811175).
2.  Uruchom program Microsoft Azure programu PowerShell.
3.  Uruchom **AzureAccount Dodaj** do uwierzytelnienia do subskrypcji usługi Azure. Po wyświetleniu monitu wprowadź samą nazwę użytkownika i hasło, którego używasz, aby zalogować się do portalu Azure.  
4.  Uruchom **Get-AzureSubscription** , aby wyświetlić listę subskrypcje skojarzonego z kontem użytkownika. 
5.  Uruchom **AzureSubscription wybierz** i określ nazwę subskrypcji lub identyfikator korzystać w konsoli programu PowerShell.

Gratulacje, konsola Azure programu PowerShell jest skonfigurowana i gotowa do użycia. Pamiętaj, że musisz repeate kroki od 2 do 5 każdym uruchomieniu konsoli Azure programu PowerShell.  

## <a name="create-a-cloud-collection"></a>Tworzenie zbioru chmury
--------------------
To proste, uruchom następujące polecenie:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Powyższe polecenie automatycznie publikuje aplikacji usługi Microsoft Office 365 (programu Excel, OneNote, Outlook, PowerPoint, Visio i Word).

Tworzenie zbioru może potrwać 30 minut lub dłużej. W związku z tym to polecenie zwraca identyfikator śledzenia, którego można używać w następujący sposób:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Po zakończeniu kolekcji użytkowników można dodać do kolekcji przy użyciu następującego polecenia:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

I gotowe! Tego użytkownika muszą mieć możliwość łączenia się z aplikacją przy użyciu klienta Azure RemoteApp znaleźć [w tym miejscu](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Dostępne polecenia cmdlet
Istnieje wiele innych poleceń mamy, dokumentacji ich będzie wkrótce biuletyny:

Podstawowe zbioru RemoteApp polecenia cmdlet: 

- Nowy AzureRemoteAppCollection
- Get-AzureRemoteAppCollection
- Ustawianie AzureRemoteAppCollection
- Aktualizacja AzureRemoteAppCollection
- Usuń AzureRemoteAppCollection
- Dodawanie AzureRemoteAppUser
- Get-AzureRemoteAppUser
- Usuń AzureRemoteAppUser
- Get-AzureRemoteAppSession
- Rozłączanie AzureRemoteAppSession
- Wywołania AzureRemoteAppSessionLogoff
- Wyślij AzureRemoteAppSessionMessage
- Get-AzureRemoteAppProgram
- Get-AzureRemoteAppStartMenuProgram
- Publikowanie AzureRemoteAppProgram
- Cofanie publikowania AzureRemoteAppProgram
- Get-AzureRemoteAppCollectionUsageDetails
- Get-AzureRemoteAppCollectionUsageSummary
- Get-AzureRemoteAppPlan

Funkcja RemoteApp wirtualną sieć polecenia cmdlet:

- Nowy AzureRemoteAppVNet
- Get-AzureRemoteAppVNet
- Ustawianie AzureRemoteAppVNet
- Usuń AzureRemoteAppVNet
- Get-AzureRemoteAppVpnDevice
- Get - AzureRemoteAppVpnDeviceConfigScript
- Resetuj AzureRemoteAppVpnSharedKey

Obraz szablonu RemoteApp polecenia cmdlet:

- Nowy AzureRemoteAppTemplateImage
- Get-AzureRemoteAppTemplateImage
- Zmień nazwę AzureRemoteAppTemplateImage
- Usuń AzureRemoteAppTemplateImage

Inne polecenia cmdlet RemoteApp:

- Get-AzureRemoteAppLocation
- Get-AzureRemoteAppWorkspace
- Ustawianie AzureRemoteAppWorkspace
- Get-AzureRemoteAppOperationResult
 
