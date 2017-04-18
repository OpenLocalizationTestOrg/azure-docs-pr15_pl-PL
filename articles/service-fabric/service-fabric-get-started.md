<properties
   pageTitle="Konfigurowanie środowiska programowania | Microsoft Azure"
   description="Zainstaluj środowisko uruchomieniowe, SDK i narzędzia i utworzyć klaster rozwoju lokalnego. Po zakończeniu instalacji ten użytkownik będzie gotowy do tworzenia aplikacji."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Przygotowywanie środowiska programowania

> [AZURE.SELECTOR]
-[Systemu Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Do tworzenia i uruchamiania [aplikacji tkaninie usługi Azure] [ 1] na komputerze rozwoju zainstaluj środowisko uruchomieniowe, SDK i narzędzi. Trzeba również włączyć wykonanie skrypty środowiska Windows PowerShell dostępny w zestawie SDK.

## <a name="prerequisites"></a>Wymagania wstępne
### <a name="supported-operating-system-versions"></a>Obsługiwane wersje systemów operacyjnych
Dla rozwoju są obsługiwane z następujących wersji systemu operacyjnego:

- Windows 7
- Windows 8 i Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 domyślnie zawiera tylko Windows PowerShell 2.0. Polecenia cmdlet programu PowerShell tkaninie usługi wymaga programu PowerShell 3.0 lub nowszy. Można [pobrać program Windows PowerShell 5.0] [ powershell5-download] z Center Download firmy Microsoft.

## <a name="install-the-runtime-sdk-and-tools"></a>Zainstaluj środowisko uruchomieniowe, SDK i narzędzia

Instalator platformy sieci Web oferuje dwie konfiguracje rozwoju tkaninie usługi:

- [Zainstaluj środowisko uruchomieniowe usługi tkaninie, SDK i narzędzia Visual Studio 2015 r (wymaga programu Visual Studio 2015 aktualizacji 2 lub nowszy)][full-bundle-vs2015]
- [Instalowanie obsługi tkaninie usługi i tylko SDK (nie narzędzia Visual Studio)][core-sdk]

## <a name="enable-powershell-script-execution"></a>Włącz wykonywanie skryptu programu PowerShell

Usługa tkaninie używa skrypty środowiska Windows PowerShell do tworzenia klaster lokalny rozwój i wdrażanie aplikacji z programu Visual Studio. Domyślnie system Windows blokuje skrypty uruchamiania. Aby je włączyć, należy zmodyfikować zasad wykonanie programu PowerShell. Otwieranie programu PowerShell jako administrator i wpisz następujące polecenie:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Następne kroki
Teraz, gdy zakończeniu górę środowiska programowania Rozpocznij tworzenie i uruchamianie aplikacji.

- [Tworzenie pierwszej aplikacji usługi tkaninie w programie Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
- [Dowiedz się, jak wdrażać i zarządzanie aplikacjami w klastrze lokalne](service-fabric-get-started-with-a-local-cluster.md)
- [Dowiedz się więcej o modelach programowania: zaufanego usług i zaufanego uczestników](service-fabric-choose-framework.md)
- [Zapoznaj się z usługi tkaninie przykłady kodu na GitHub](https://aka.ms/servicefabricsamples)
- [Wizualizowanie klaster przy użyciu Eksploratora tkaninie usługi](service-fabric-visualizing-your-cluster.md)
- [Wykonaj ścieżka nauki tkaninie usługi, aby uzyskać ogólne wprowadzenie do platformy](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Usługa tkaninie kampanii strony"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "W PORÓWNANIU Z RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Łącze w PORÓWNANIU z WebPI 2015 r."
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Łącze Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Łącze SDK WebPI Core"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
