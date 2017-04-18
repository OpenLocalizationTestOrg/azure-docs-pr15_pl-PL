<properties
   pageTitle="Zgłaszanie i sprawdzanie kondycji z tkaninie usługi Azure | Microsoft Azure"
   description="Dowiedz się, jak wysyłać raporty dotyczące kondycji w kodzie usługi i sposobu sprawdzania kondycji usługi za pomocą narzędzi monitorowania kondycji dostępnych tkaninie usługi Azure."
   services="service-fabric"
   documentationCenter=".net"
   authors="toddabel"
   manager="mfussell"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/06/2016"
   ms.author="toddabel"/>

# <a name="report-and-check-service-health"></a>Zgłaszanie i sprawdzanie kondycji usługi
Usług występują problemy, możliwość odpowiadanie na nie i poprawianie zdarzeń i dostawie zależy Ci możliwość szybkiego wykrywania problemów. Jeśli możesz raportować problemy i błędy do Menedżera kondycji tkaninie usługi Azure w kodzie usługi, możesz użyć standardowego kondycji narzędzi dostępnych tkaninie usługi, aby sprawdzić stan kondycji monitorowania.

Istnieją dwa sposoby zgłosić kondycja usługi:

- Używanie obiektów [partycją](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.aspx) lub [CodePackageActivationContext](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.aspx) .  
Możesz użyć `Partition` i `CodePackageActivationContext` obiekty do raportu kondycji elementy, które są częścią bieżącego kontekstu. Na przykład kod, który jest uruchamiany jako część replice zgłosić kondycji tylko w tej replice, partycją należący do i aplikacji, która jest częścią.

- Używanie `FabricClient`.   
Możesz użyć `FabricClient` zdrowia raportu z kod usługi, jeśli klaster nie jest [bezpieczny](service-fabric-cluster-security.md) lub jeśli usługa jest uruchomiona z uprawnieniami administratora. To nie będzie w większości rzeczywistych scenariuszy. Z `FabricClient`, możesz to zgłosić kondycji na osobę, który jest częścią klaster. Najlepiej, jeśli jednak kod usługi należy wysyłać tylko raportów, które są związane z własną kondycji.

W tym artykule opisano przykład raporty o kondycji z kod usługi. Również w przykładzie pokazano, jak można narzędzi, które znajdują się tkaninie usługi Aby sprawdzić stan kondycji. Ten artykuł ma być krótkie wprowadzenie do monitorowania możliwości usługi tkaninie zdrowia. Aby uzyskać więcej szczegółowych informacji można czytać serii szczegółowo artykułów dotyczących kondycji, które zaczynają się od łącza na końcu tego artykułu.

## <a name="prerequisites"></a>Wymagania wstępne
Musi być zainstalowane następujące elementy:

   * Visual Studio 2015 r.
   * Usługa tkaninie SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Aby utworzyć klaster lokalny bezpiecznego deweloperów
- Otwórz programu PowerShell z uprawnieniami administratora i uruchom następujące polecenia.

![Polecenia, które pokazująca, jak utworzyć klaster bezpiecznego deweloperów](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Aby wdrożyć aplikację i sprawdzanie jego kondycji

1. Otwórz program Visual Studio jako administrator.

2. Tworzenie projektu za pomocą szablonu **Stateful usługi** .

    ![Tworzenie aplikacji usługi tkaninie z usługą Stateful](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)

3. Naciśnij klawisz **F5** , aby uruchomić aplikację w trybie debugowania. Aplikacja zostanie wdrożony w klaster lokalny.

4. Po uruchomieniu aplikacji kliknij prawym przyciskiem myszy ikonę Menedżer lokalnej klaster w obszarze powiadomień i wybierz pozycję **Zarządzaj klaster lokalny** z menu skrótów, aby otworzyć Eksploratora tkaninie usługi.

    ![Otwórz Eksploratora tkaninie usługi z obszaru powiadomień](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)

5. Kondycja aplikacji powinny być wyświetlane jak ten obraz. W tym momencie aplikacja powinna być prawidłowy bez błędów.

    ![Prawidłowy aplikacji w Eksploratorze tkaninie usługi](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)

6. Możesz również sprawdzić stan przy użyciu programu PowerShell. Możesz użyć ```Get-ServiceFabricApplicationHealth``` sprawdzania kondycji aplikacji, dzięki czemu można użyć ```Get-ServiceFabricServiceHealth``` sprawdzania kondycji usługi. Raport dotyczący kondycji dla tej samej aplikacji w programie PowerShell znajduje się w tym obrazu.

    ![Prawidłowy aplikacji w programie PowerShell](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Aby dodać niestandardowe kondycji zdarzeń w kodzie usługi
Szablony projektów tkaninie usługi w programie Visual Studio zawierają przykładowy kod. Poniższe kroki pokazują, jak można zgłosić zdarzenia niestandardowe kondycji w kodzie usługi. Takie raporty automatycznie pojawi się w standardowe narzędzia kondycji monitorowania, że znajdują się tkaninie usług, takich jak usługa tkaninie Eksploratora, widok Azure kondycji portalu i programu PowerShell.

1. Otwórz aplikację, która utworzyła wcześniej w programie Visual Studio lub tworzenie nowej aplikacji za pomocą szablonu programu Visual Studio **Stateful usługi** .

2. Otwórz plik Stateful1.cs i Znajdź `myDictionary.TryGetValueAsync` połączenia `RunAsync` metody. Widać, że ta metoda zwraca `result` zawierającego bieżącą wartość licznika, ponieważ klucza reguł w tej aplikacji jest liczbą uruchomiony. Jeżeli rzeczywistej aplikacji i Brak wyników przedstawione w błąd, czy chcesz oznaczyć flagą to zdarzenie.

3. Aby zgłosić zdarzenie kondycji przy braku wyników reprezentuje błąd, Dodaj następujące czynności.

    . Dodawanie `System.Fabric.Health` nazw do pliku Stateful1.cs.

    ```csharp
    using System.Fabric.Health;
    ```

    b. Dodaj następujący kod po `myDictionary.TryGetValueAsync` połączeń

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Firma Microsoft raportować kondycji replice, ponieważ jest zgłaszane z akumulującej stan usługi. `HealthInformation` Parametru przechowuje informacje o problemie kondycji, który jest zgłaszane.

    Jeśli utworzono stateless usługi, należy użyć następującego kodu

    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```

4. Jeśli Twoja usługa działa z uprawnieniami administratora lub jeśli klaster nie jest [bezpieczny](service-fabric-cluster-security.md), możesz również użyć `FabricClient` zdrowia raportu, jak pokazano w następującej procedurze.  

    . Tworzenie `FabricClient` wystąpienia po `var myDictionary` deklaracji.

    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```

    b. Dodaj następujący kod po `myDictionary.TryGetValueAsync` połączeń.

    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.ServiceInitializationParameters.PartitionId,
            this.ServiceInitializationParameters.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```

5. Załóżmy symulować ten błąd i widzisz go są wyświetlane w narzędzi do monitorowania kondycji. Aby symulować awarię, komentarz w pierwszym wierszu kondycji kod, który dodano wcześniej raportowania. Po skomentować pierwszego wiersza, kod będzie wyglądać jak w następującym przykładzie.

    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
 Kod, jaki będzie teraz uruchomienie ten raport dotyczący kondycji zawsze `RunAsync` wykonuje. Po wprowadzeniu zmian naciśnij klawisz **F5** , aby uruchomić aplikację.

6. Po uruchomieniu aplikacji otwórz Eksploratora tkaninie usługi, aby sprawdzić kondycję aplikacji. Tym razem usługi tkaninie Explorer zostanie wyświetlona aplikacja jest nieprawidłowe. Jest to ze względu na błąd, który został zgłoszony z kodu, że dodana wcześniej.

    ![Nieprawidłowe aplikacji w Eksploratorze tkaninie usługi](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)

7. Po wybraniu replice podstawowy w widoku drzewa Eksploratora tkaninie usługi pojawi się **Kondycję** zbyt wskazuje błąd. Eksplorator tkaninie usługi są wyświetlane szczegóły raportu kondycji, które zostały dodane do `HealthInformation` parametru w kodzie. Widać, tym samym raporty dotyczące kondycji programu PowerShell i Azure portal.

    ![Kondycja replice w Eksploratorze tkaninie usługi](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Ten raport pozostanie w Menedżerze kondycji aż jest on zastępowany przez innego raportu lub do momentu usunięcia tej replice. Ponieważ firma Microsoft nie ustawił `TimeToLive` dla tego raportu kondycji w `HealthInformation` obiektu, nigdy nie wygasa raportu.

Zaleca się, że kondycji należy podać na poziomie najbardziej szczegółowego, czyli w tym przypadku replice. Możesz również zgłosić kondycji na `Partition`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Do pozycji kondycja raportu na `Application`, `DeployedApplication`, i `DeployedServicePackage`, za pomocą `CodePackageActivationContext`.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Następne kroki
[Głębokości dive na tkaninie usługi kondycji](service-fabric-health-introduction.md)
