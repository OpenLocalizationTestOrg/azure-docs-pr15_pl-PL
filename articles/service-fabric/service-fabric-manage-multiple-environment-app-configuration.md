<properties
   pageTitle="Zarządzanie wielu środowiskach w tkaninie usługi | Microsoft Azure"
   description="Aplikacje usługi tkaninie można uruchamiać na klastrów o rozmiarze z jednego komputera do tysięcy komputerów. W niektórych przypadkach można skonfigurować aplikację inaczej w tych różnych środowiskach. W tym artykule opisano, jak Definiowanie parametrów innej aplikacji na środowiska."
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
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Zarządzanie parametry aplikacji w wielu środowiskach

Możesz utworzyć klastrów tkaninie usługi Azure za pomocą dowolnego miejsca z jeden-do-wielu tysięcy komputerów. Gdy plików binarnych aplikacji może zostać uruchomiony bez zmian przez ten szeroką gamę środowiskach, często należy skonfigurować aplikację w inny sposób, w zależności od liczby komputerów, który jest instalowany do.

Prosty przykład, należy rozważyć, czy `InstanceCount` stateless usługi. Po uruchomieniu aplikacji platformy Azure, zazwyczaj można ten parametr jest ustawiony na wartość specjalne "-1". Dzięki temu, że w każdym węźle w klastrze działa z usługą. Jednak tej konfiguracji nie jest odpowiedni dla klastrów pojedynczego komputera, ponieważ nie może mieć wiele procesów nasłuchują na tym samym punktu końcowego na jednym komputerze. Zamiast tego zwykle zostanie ustawiona `InstanceCount` "1".

## <a name="specifying-environment-specific-parameters"></a>Określanie parametrów specyficzne dla środowiska

Rozwiązanie tego problemu konfiguracji to zbiór parametryczne domyślnych usług i aplikacji parametru pliki, które wypełnij te wartości parametru dla danego środowiska. Domyślne usługi i parametry aplikacji są skonfigurowane w manifesty aplikacji i usług. Definicja schematu pliki ServiceManifest.xml i ApplicationManifest.xml jest zainstalowany wraz z SDK tkaninie usługi i narzędzia do *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Domyślne usługi

Aplikacje usługi tkaninie składają się zbioru wystąpień usługi. W trakcie pozwala utworzyć pustą aplikację, a następnie utworzyć dynamiczne wszystkich wystąpień usługi większość aplikacji ma zbiór usług podstawowych, które powinny być zawsze tworzone podczas aplikacji zostanie uruchomiony. Te są nazywane "usługi domyślne". Są one określane w oczywiste aplikacji, z symbolami zastępczymi konfiguracji-środowisko zawarte w nawiasach kwadratowych:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Każdy nazwane parametry należy zdefiniować w elemencie parametrów manifest aplikacji:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Atrybut DefaultValue określa wartość, która ma być używany w przypadku braku parametru więcej specyficzne dla danego środowiska.

>[AZURE.NOTE] Nie wszystkie parametry wystąpienia usługi są odpowiednie dla konfiguracji jednego środowiska. W powyższym przykładzie wartości LowKey i HighKey systemu podziału usługi są zdefiniowane dla wszystkich wystąpień usługi od zakresu partycją jest funkcją domeny danych, nie środowiska.


### <a name="per-environment-service-configuration-settings"></a>Ustawienia konfiguracji usługi dla środowiska

[Model aplikacji usługi tkaninie](service-fabric-application-model.md) umożliwia korzystanie z usług do uwzględnienia pakietach konfiguracji, które zawierają niestandardowe par klucz wartość, które można odczytać w czasie wykonywania. Wartości tych ustawień można również zróżnicowane środowisko, określając `ConfigOverride` w manifeście aplikacji.

Załóżmy, że zostały następujące ustawienie w pliku Config\Settings.xml `Stateful1` usługi:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Aby zastąpić tę wartość dla pary określonej aplikacji i środowiska, Utwórz `ConfigOverride` podczas importowania manifestu usługi w manifeście aplikacji.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Ten parametr następnie można skonfigurować środowisko, jak pokazano powyżej. Możesz to zrobić deklarowanie go w sekcji Parametry manifest aplikacji i określanie wartości specyficzne dla środowiska w plikach parametru aplikacji.

>[AZURE.NOTE] W przypadku ustawienia konfiguracji usługi, dostępne są trzy miejsca, w którym można ustawić wartości klucza: pakiet Konfiguracja usługi, manifest aplikacji i pliku parametru aplikacji. Tkaninie usługi będzie zawsze wybierz pozycję z pliku parametru aplikacji pierwszy (Jeśli określony), następnie manifest aplikacji, a na końcu pakietu konfiguracji.


### <a name="application-parameter-files"></a>Pliki aplikacji parametru

Projekt aplikacji usługi tkaninie może zawierać jeden lub więcej plików parametr aplikacji. Każdy z nich definiuje określonych wartości parametrów zdefiniowanych w manifeście aplikacji:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Domyślnie nowej aplikacji zawiera dwa parametru pliki aplikacji o nazwie Local.xml i Cloud.xml:

![Pliki aplikacji parametr w Eksploratorze rozwiązań][app-parameters-solution-explorer]

Aby utworzyć nowy plik parametr, po prostu skopiuj i Wklej istniejącej i nadać jej nową nazwę.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Identyfikowanie specyficzne dla środowiska parametry podczas wdrażania

W czasie rozmieszczania należy wybrać plik odpowiedni parametr, aby zastosować z aplikacją. Można to zrobić za pośrednictwem okna dialogowego Publikuj w programie Visual Studio lub przy użyciu programu PowerShell.

### <a name="deploy-from-visual-studio"></a>Wdrażanie z programu Visual Studio

Możesz wybrać z listy dostępne parametru plików podczas publikowania aplikacji w programie Visual Studio.

![Wybierz plik parametr w oknie dialogowym Publikowanie][publishdialog]

### <a name="deploy-from-powershell"></a>Wdrażanie z programu PowerShell

`Deploy-FabricApplication.ps1` Skrypt programu PowerShell zawarte w szablonie projektu aplikacji akceptuje profilu publikowania jako parametr i PublishProfile zawiera odwołanie do pliku parametrów aplikacji.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o niektórych podstawowych koncepcji, które zostały omówione w tym temacie, zobacz [Omówienie kwestii technicznych tkaninie usługi](service-fabric-technical-overview.md). Aby uzyskać informacje o funkcjach zarządzania innych aplikacji, które są dostępne w programie Visual Studio zobacz [Zarządzanie tkaninie usługi aplikacji w programie Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
