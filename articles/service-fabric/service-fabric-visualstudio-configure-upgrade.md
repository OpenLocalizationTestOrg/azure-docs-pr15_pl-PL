<properties
   pageTitle="Konfigurowanie uaktualnienia aplikacji usługi tkaninie | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować ustawienia uaktualniania aplikacji usługi tkaninie przy użyciu programu Microsoft Visual Studio."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Konfigurowanie uaktualnienia aplikacji usługi tkaninie w programie Visual Studio

Program Visual Studio tools tkaninie usługi Azure obsługiwać uaktualnienia do opublikowania do klastrów lokalną lub zdalną. Istnieją dwie zalety uaktualnienie aplikacji do nowszej wersji zamiast zastępowania aplikacji podczas testowania i debugowania:

- Dane aplikacji nie zostaną utracone podczas uaktualniania.
- Dostępność pozostaje wysoki, nie będzie przerwy usługi podczas uaktualniania jeśli znajdują się za mało wystąpień usługi rozciągnąć uaktualnienia domen.

Testy mogą być uruchamiane na aplikacji, gdy jest uaktualniany.

## <a name="parameters-needed-to-upgrade"></a>Parametry potrzebne do uaktualnienia

Dostępne są dwa typy rozmieszczania: regularnego lub uaktualnienia. Zwykła wdrożenia usuwa poprzednie informacje na temat wdrażania, a dane w klastrze, podczas uaktualniania wdrożenia zachowuje go. Po uaktualnieniu aplikacją usługi tkaninie w programie Visual Studio, należy podać parametry uaktualnienia aplikacji i kondycji zaznaczenia zasady. Parametry uaktualnienia aplikacji ograniczyć uaktualniania podczas zasady wyboru kondycji określają, czy uaktualnienie zakończyło się pomyślnie. Zobacz [uaktualnienia aplikacji usługi tkaninie: uaktualnianie parametrów](service-fabric-application-upgrade-parameters.md) uzyskać więcej szczegółowych informacji.

Istnieją trzy tryby uaktualnienia: *monitorowane*, *UnmonitoredAuto*i *UnmonitoredManual*.

  - Uaktualnianie monitorowane zautomatyzowanie uaktualnienia i sprawdzanie kondycji aplikacji.

  - Uaktualnienie UnmonitoredAuto zautomatyzowanie uaktualnienie, ale pomija sprawdzanie kondycji aplikacji.

  - Po wykonaniu uaktualnienia UnmonitoredManual należy ręcznie uaktualnić każdej uaktualnienia domeny.

Każdy tryb uaktualniania wymaga różnymi zestawami parametrów. Sprawdź [Parametry uaktualnienia aplikacji](service-fabric-application-upgrade-parameters.md) , aby dowiedzieć się więcej o dostępnych opcji uaktualnienia.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Uaktualnianie aplikacji usługi tkaninie w programie Visual Studio

Jeśli korzystasz z narzędzia Visual Studio usługi tkaninie, aby uaktualnić aplikację tkaninie usługi, można określić proces publikowania do uaktualnienia, a nie na zwykłą wdrożenia, zaznaczając pole wyboru **uaktualnienie aplikacji** .

### <a name="to-configure-the-upgrade-parameters"></a>Aby skonfigurować parametry uaktualnienia

1. Kliknij przycisk **Ustawienia** obok pola wyboru. Zostanie wyświetlone okno dialogowe **Edytuj parametry uaktualnienia** . Okno dialogowe **Edytuj parametry uaktualnienie** obsługuje tryby uaktualnienia monitorowane, UnmonitoredAuto i UnmonitoredManual.

2. Wybierz tryb uaktualniania, którego chcesz użyć, a następnie wypełnij siatki parametru.

    Każdy parametr ma wartości domyślne. Parametr opcjonalny *DefaultServiceTypeHealthPolicy* trwa wprowadzania tabeli skrótu. Oto przykład formatu tabeli wprowadzania skrótu *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* jest inny parametr opcjonalny, który ma wprowadzania tabeli mieszania w następującym formacie:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Oto przykład rzeczywistych:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Po wybraniu trybu uaktualniania UnmonitoredManual konsoli programu PowerShell, aby kontynuować i Zakończ proces uaktualniania trzeba uruchomić ręcznie. Zapoznaj się z [uaktualnienia aplikacji usługi tkaninie: tematy dotyczące zaawansowanego](service-fabric-application-upgrade-advanced.md) Aby dowiedzieć się, jak ręcznego uaktualnienia działa.

## <a name="upgrade-an-application-by-using-powershell"></a>Uaktualnianie aplikacji przy użyciu programu PowerShell

Za pomocą poleceń cmdlet środowiska PowerShell Aby uaktualnić aplikację usługi tkaninie. Aby uzyskać szczegółowe informacje, zobacz [aplikacji usługi tkaninie uaktualnienie samouczka](service-fabric-application-upgrade-tutorial.md) i [Start ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) .

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Określanie zasad wyboru kondycji w pliku manifestu aplikacji

Każdej usługi w aplikacji usługi tkaninie może mieć własny parametry zasad kondycji, które zastępują domyślne wartości. Można udostępnić te wartości parametrów w pliku manifestu aplikacji.

W poniższym przykładzie pokazano, jak stosowanie zasad wyboru kondycji unikatowe dla każdej usługi manifest aplikacji.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat wdrażania aplikacji zobacz temat [Deploy istniejącą aplikację na tkaninie usługi Azure](service-fabric-deploy-existing-app.md).
