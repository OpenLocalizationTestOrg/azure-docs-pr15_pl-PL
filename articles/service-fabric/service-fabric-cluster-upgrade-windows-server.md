<properties
   pageTitle="Uaktualnianie klaster autonomicznej usługi tkaninie w systemie Windows Server | Microsoft Azure"
   description="Uaktualnianie kodu tkaninie usługi i/lub konfiguracji, która jest uruchomiony klaster tkaninie usługi autonomicznej, wraz z ustawieniem klaster tryb aktualizacji"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Uaktualnianie klaster tkaninie usługi autonomicznej w systemie Windows Server

> [AZURE.SELECTOR]
- [Klaster Azure](service-fabric-cluster-upgrade.md)
- [Klaster autonomicznego](service-fabric-cluster-upgrade-windows-server.md)

Dla każdego nowoczesnego systemu projektowanie na możliwość jest kluczem do osiągnięcia sukcesu długoterminowe produktu. Klaster tkaninie usługi jest zasób, którego jesteś właścicielem. W tym artykule opisano, jak można upewnić się, że zawsze uruchamiane w klastrze obsługiwanych wersji tkaninie usługi kod i konfiguracji.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Sterowanie wersji tkaninie uruchamianej na klaster

Możesz skonfigurować klaster pobieranie aktualizacji tkaninie usług, gdy firma Microsoft udostępnia nową wersję i wybierz pozycję, aby wybrać wersję obsługiwane tkaninie potrzebne klaster w. 

W tym celu ustawienie konfiguracji klaster "fabricClusterAutoupgradeEnabled" na wartość PRAWDA lub FAŁSZ.


>[AZURE.NOTE] Upewnij się zachować klaster zawsze używać obsługiwanej wersji usługi tkaninie. Jak i firma Microsoft o nowej wersji materiału usługi, poprzedniej wersji jest oznaczony w celu obsługi po co najmniej 60 dni od tej daty. Nowe wersje są wprowadzona [w blogu zespołu tkaninie usługi](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Następnie wybierz znajduje się nowej wersji. 


Można też uaktualnić klaster do nowej wersji, tylko wtedy, gdy korzystasz z konfigurację węzeł produkcji stylu, gdzie przydzielonego każdy węzeł tkaninie usługi na osobne fizycznej lub maszyn wirtualnych. Jeśli masz klastrze rozwoju w przypadku, gdy istnieje więcej niż jedna usługa tkaninie węzłów na jednej fizycznej lub maszyn wirtualnych, należy usunąć klaster i utwórz go ponownie z nową wersją.


Istnieją dwa różne przepływy pracy uaktualniania klaster do r lub wersja tkaninie obsługiwanej usługi. Jeden dla klastrów zawierających łączności z programem automatycznie Pobierz najnowszą wersję, a drugi dla klastrów, które są Pobierz najnowszą wersję usługi tkaninie braku łączności.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Uaktualnianie klastrów z łącznością z pobrać najnowsze kod i konfiguracji 

Wykonaj poniższe kroki, aby uaktualnić klaster do obsługiwanej wersji Jeśli węzły klaster masz połączenie z Internetem do [http://download.microsoft.com](http://download.microsoft.com) 

W przypadku klastrów zawierających łączności z programem [http://download.microsoft.com](http://download.microsoft.com)firma Microsoft okresowo sprawdzał dostępność nowych wersji tkaninie usługi.


Po udostępnieniu nowej wersji tkaninie usługi pakietu pobraniu lokalnie z klastrem i obsługi administracyjnej uaktualnienia. Ponadto informowanie klienta nowa wersja systemu umieszcza ostrzeżenie o kondycji jawne klaster podobny do następującego:

"Klaster wersji bieżącej [wersja #], [Data] kończy się pomocy technicznej.", gdy klaster działa najnowszej wersji, ostrzeżenia zniknie.


#### <a name="cluster-upgrade-workflow"></a>Klaster uaktualnienia przepływu pracy.
 
Po wyświetleniu ostrzeżenia o kondycji klaster, należy wykonać następujące czynności:

1. Połącz się z klastrem z dowolnego komputera, na którym ma dostęp do wszystkich komputerów, na których są wyświetlane jako węzły w klastrze administrator. Komputer, uruchamianego tego skryptu nie ma być częścią klaster

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Uzyskiwanie listy materiału usługi wersji, które można też uaktualnić do

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    wynik powinna pobierać podobnie do następującej:

    ![Uzyskiwanie tkaninie wersji][getfabversions]

3. Rozpoczynanie uaktualnienia klaster do jednej wersji, które jest dostępne za pomocą [programu PowerShell Start ServiceFabricClusterUpgrade cmd](https://msdn.microsoft.com/library/mt125872.aspx)

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Można monitorować postęp uaktualnienia usługi tkaninie Eksploratora lub, uruchamiając następujące polecenie powłoki power

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Po ustaleniu problemy, które spowodowały wycofywania, musisz zainicjować aktualizację ponownie, wykonując te same kroki jako przed.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Uaktualnianie klastrów z <U>brak łączności</u> Aby pobrać najnowszą kod i konfiguracji

Wykonaj poniższe kroki, aby uaktualnić klaster do obsługiwanej wersji Jeśli klaster węzły **nie masz** połączenia internetowego do [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Jeśli używasz klaster, który nie jest połączenie internetowe, konieczne będzie monitorowanie usługi blogu zespołu tkaninie Uzyskiwanie powiadomienia o nowej wersji. System **nie** umieść ostrzeżenia dotyczące kondycji klaster alert go.  

1. Modyfikowanie konfiguracji klaster, aby ustawić następujące właściwości ma wartość FAŁSZ.

        "fabricClusterAutoupgradeEnabled": false,

i rozpocząć uaktualnianie konfiguracji. Zapoznaj się z [cmd PS Start ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) , aby uzyskać informacje dotyczące sposobu użycia. Wersja manifestu klaster jest wersji, które mają w clusterConfig.JSON. Pamiętaj je zaktualizować przed rozpoczęcie wyłączanie uaktualnienie konfiguracji.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Klaster uaktualnienia przepływu pracy.
 


1. Pobierz najnowszą wersję pakietu z dokumentu [Utwórz usługi tkaninie klaster dla systemu windows server](service-fabric-cluster-creation-for-windows-server.md) 


1. Połącz się z klastrem z dowolnego komputera, na którym ma dostęp do wszystkich komputerów, na których są wyświetlane jako węzły w klastrze administrator. Komputer, uruchamianego tego skryptu nie ma być częścią klaster 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Skopiuj pobrany pakiet w magazynie obraz klaster.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Zarejestruj się w pakiecie skopiowany 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Rozpoczynanie uaktualnienia klaster do jednej z dostępnych wersji. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Można monitorować postęp uaktualnienia usługi tkaninie Eksploratora lub, uruchamiając następujące polecenie powłoki power

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Po ustaleniu problemy, które spowodowały wycofywania, musisz zainicjować aktualizację ponownie, wykonując te same kroki jako przed.



## <a name="next-steps"></a>Następne kroki
- Dowiedz się, jak dostosowywać niektóre [Ustawienia tkaninie klaster tkaninie usługi](service-fabric-cluster-fabric-settings.md)
- Dowiedz się, jak [i zmniejszanie](service-fabric-cluster-scale-up-down.md) przeskalować klaster
- Więcej informacji na temat [uaktualnienia aplikacji](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
