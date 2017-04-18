<properties
   pageTitle="Rozwiązywanie problemów z lokalnej usługi Klaster konfigurowania | Microsoft Azure"
   description="W tym artykule opisano, jak zestaw sugestie dotyczące rozwiązywania problemów z klastrem rozwoju lokalnego"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="troubleshoot-your-local-development-cluster-setup"></a>Rozwiązywanie problemów z ustawień klaster rozwoju lokalnego

Jeśli napotkasz problem podczas interakcji z lokalnym klaster rozwoju tkaninie usługi Azure Przejrzyj następujące sugestie dotyczące potencjalnego rozwiązania.

## <a name="cluster-setup-failures"></a>Klaster błędów instalacji

### <a name="cannot-clean-up-service-fabric-logs"></a>Nie można wyczyścić dzienników tkaninie usługi

#### <a name="problem"></a>Problem

Uruchamiając skrypt DevClusterSetup, pojawi się komunikat o błędzie w następujący sposób:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Rozwiązanie

Zamknięcie bieżącego okna programu PowerShell i otwieranie nowego okna programu PowerShell jako administrator. Teraz powinno być możliwe pomyślne uruchomienie skryptu.

## <a name="cluster-connection-failures"></a>Klaster błędy połączenia

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Polecenia cmdlet programu PowerShell tkaninie usług nie zostaną rozpoznane w programie PowerShell Azure

#### <a name="problem"></a>Problem

Jeśli próbujesz uruchomić dowolnego z poleceń cmdlet programu PowerShell tkaninie usług, takich jak `Connect-ServiceFabricCluster` w oknie programu PowerShell Azure go nie powiedzie się, informujący, że nie został rozpoznany polecenia cmdlet. Przyczyną jest używany Azure programu PowerShell 32-bitową wersję programu Windows PowerShell (nawet w 64-bitowej wersji systemu operacyjnego), cmdlet usługi tkaninie działają tylko w środowiskach 64-bitowej.

#### <a name="solution"></a>Rozwiązanie

Polecenia cmdlet usługi tkaninie są zawsze uruchamiane bezpośrednio z programu Windows PowerShell.

>[AZURE.NOTE] Najnowszą wersję programu Azure PowerShell nie tworzy skrótu specjalny, to nie jest już powinna zostać wykonana.

### <a name="type-initialization-exception"></a>Typ wyjątku inicjowania

#### <a name="problem"></a>Problem

Gdy łączysz się z klastrem w programie PowerShell, zostanie wyświetlony błąd TypeInitializationException dla System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Rozwiązanie

Do zmiennej ścieżka nie została poprawnie ustawiona podczas instalacji. Wylogowywanie się z systemu Windows i zaloguj się ponownie. Spowoduje to w pełni odświeżenie ścieżce.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Klaster połączenia nie powiodło się "Obiekt jest zamknięty"

#### <a name="problem"></a>Problem

Połączenie do ServiceFabricCluster Połącz kończy się niepowodzeniem z powodu błędu w następujący sposób:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Rozwiązanie

Zamknięcie bieżącego okna programu PowerShell i otwieranie nowego okna programu PowerShell jako administrator. Teraz należy połączyć.

### <a name="fabric-connection-denied-exception"></a>Tkaninie wyjątku odmowa połączenia

#### <a name="problem"></a>Problem

Podczas debugowania z programu Visual Studio, zostanie wyświetlony komunikat o błędzie FabricConnectionDeniedException.

#### <a name="solution"></a>Rozwiązanie

Ten błąd występuje zwykle w przypadku spróbuj do spróbuj uruchomić hosta usługi przetwarzania ręcznie, zamiast zezwolenia na środowisko uruchomieniowe usługi tkaninie go uruchomić dla Ciebie.

Upewnij się, nie ma żadnych projektów usługi Ustaw jako uruchamiania projektów w rozwiązaniu. Tylko projektów aplikacji usługi tkaninie należy skonfigurować jako projekty uruchamiania.

>[AZURE.TIP] Jeśli po zakończeniu instalacji, lokalne klaster zaczyna działać nieprawidłowo, możesz przywrócić go przy użyciu aplikacji obszar powiadomień systemu Menedżera klaster lokalny. To usunie istniejącym klastrem i skonfigurować nową. Pamiętaj, że wszystkie wdrożony aplikacji i skojarzone dane zostaną usunięte.

## <a name="next-steps"></a>Następne kroki

- [Opis i rozwiązywanie problemów z klaster przy użyciu raportów dotyczących kondycji systemu](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Wizualizowanie klaster w Eksploratorze tkaninie usługi](service-fabric-visualizing-your-cluster.md)
