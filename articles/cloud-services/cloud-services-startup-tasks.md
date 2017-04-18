<properties 
pageTitle="Uruchamianie zadania uruchamiania w usług w chmurze Azure | Microsoft Azure" 
description="Zadania uruchamiania pomocy przygotowania środowiska chmury usługi dla aplikacji. To przedstawiono działania uruchamiania zadań i jak to zrobić" 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Jak skonfigurować i uruchomić zadania uruchamiania usługi w chmurze

Uruchamianie zadania służy do wykonywania operacji przed uruchomieniem roli. Czynności, które można wykonać to instalowania składnika, rejestrowanie składników COM, ustawienie kluczach rejestru lub rozpoczęciem długotrwałych.

>[AZURE.NOTE] Uruchamianie zadania nie są zastosowanie do maszyn wirtualnych, tylko do sieci Web usługi w chmurze i ról pracownika.

## <a name="how-startup-tasks-work"></a>Jak działają uruchamiania zadań

Uruchamianie zadania są akcje, które są pobierane przed poszczególnych ról rozpoczynają i są definiowane w pliku [ServiceDefinition.csdef] przy użyciu elementu [zadania] w elemencie [uruchamiania] . Często uruchamiania zadania są pliki wsadowe, ale można je również aplikacji konsoli lub pliki wsadowe, które uruchomić skrypty środowiska PowerShell.

Zmienne środowiska przekazywania informacji do zadania uruchamiania i magazynu lokalnego może służyć do przekazywania informacji poza zadania uruchamiania. Na przykład zmiennej środowiska można określić ścieżkę dostępu do programu, który chcesz zainstalować, a następnie można zapisywać pliki do magazynu lokalnego, który można następnie odczytywane później przez poszczególnych ról.

Zadania uruchamiania można rejestrować błędów i informacje do katalogu określonego przez zmienną środowiska **TEMP** . Podczas uruchamiania zadania, zmienną środowiska **TEMP** jest rozpoznawana jako *C:\\zasobów\\temp\\[identyfikator guid]. [ rolename]\\RoleTemp* katalogu, gdy działa w chmurze.

Uruchamiania zadań można również wykonać kilka razy między ponownego uruchamiania. Na przykład zadanie uruchomienia będzie uruchamiane zawsze, gdy odtwarza roli i odtwarzania roli może nie zawsze należy uwzględnić Uruchom ponownie komputer. Uruchamianie zadania powinny być zapisywane w sposób umożliwiający ich tak, aby uruchomić kilka razy bez problemów.

Uruchamianie zadania musi się kończyć **errorlevel** (lub kod zakończenia) zero podczas uruchamiania do wykonania. Jeśli zadanie uruchomienia kończy się niezerowy **errorlevel**, roli nie zostanie uruchomiona.


## <a name="role-startup-order"></a>Kolejność uruchamiania ról

Poniżej przedstawiono listę procedury uruchamiania ról w Azure:

1. Wystąpienie jest oznaczony jako **początkowy** i nie odbiera ruchu.

2. Wszystkie zadania uruchamiania są wykonywane zgodnie z ich atrybutami **taskType** .
    - **Proste** zadania są wykonywane synchronicznie, pojedynczo.
    - Zadania **tekstu** i **tła** są uruchamiane asynchroniczne, równolegle z zadaniem uruchamiania.  
       
    > [AZURE.WARNING] Usług IIS może nie być w pełni skonfigurowany podczas uruchamiania zadania etapu w procesie uruchamiania, dane specyficzne dla roli mogą być niedostępne. Uruchamianie zadania, które wymagają dane specyficzne dla roli, należy użyć [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).

3. Proces hosta roli jest uruchamiany i witryny zostanie utworzony w programie IIS.

4. Metoda [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) jest wywoływana.

5. Wystąpienie jest oznaczony jako **gotowy** i ruch jest kierowane do wystąpienia.

6. Metoda [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) jest wywoływana.


## <a name="example-of-a-startup-task"></a>Przykład zadania uruchamiania

Uruchamianie zadania są definiowane w pliku [ServiceDefinition.csdef] w elemencie **zadania** . Atrybut **wiersza polecenia** Określa nazwę i parametry plik wsadowy uruchamiania lub polecenia konsoli, atrybut **executionContext** określa poziom uprawnień zadania uruchamiania, a atrybut **taskType** Określa, jak zostanie wykonana zadania.

W tym przykładzie zmiennej środowiska **MyVersionNumber**jest tworzony dla zadania uruchamiania i ustawić na wartość "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

W poniższym przykładzie plik wsadowy **Startup.cmd** zapisuje wiersza "bieżącej wersji jest 1.0.0.0" do pliku StartupLog.txt w katalogu określonym przez zmienną środowiska TEMP. `EXIT /B 0` Linii gwarantuje, że zadanie uruchomienia kończy się **errorlevel** zero.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] W programie Visual Studio, **skopiować do katalogu wyjściowego** właściwości pliku partii uruchamiania powinna być równa **Kopii zawsze** należy upewnić się, że plik wsadowy uruchamiania jest prawidłowo wdrożona w projekcie Azure (**approot\\Kosza** dla ról w sieci Web i **approot** dla ról pracownika).

## <a name="description-of-task-attributes"></a>Opis atrybutów zadania

Poniżej opisano atrybuty element **zadania** w pliku [ServiceDefinition.csdef] :

**Wiersz polecenia** - Określa wiersz polecenia zadania uruchamiania:

- Polecenie z parametrami opcjonalne wiersza polecenia, który zaczyna się zadanie uruchomienia.
- Często jest to nazwa plik wsadowy cmd lub bat.
- Zadanie jest względem AppRoot\\folderu Kosza dla wdrożenia. Zmienne środowiska nie są rozwinięte w ustalaniu ścieżka i zadania. Jeśli wymagane jest rozszerzeń środowiska, można utworzyć skrypt małych cmd. wywołującego zadania uruchamiania.
- Może być aplikacji konsoli lub plik wsadowy uruchamia [skrypt programu PowerShell](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** - określa poziom uprawnień dla zadania uruchamiania. Poziom uprawnień może być ograniczone lub pełnymi:

- **ograniczone**  
Zadanie uruchomienia będzie uruchamiane przy użyciu uprawnienia równoważne z rolą. Gdy atrybutu **executionContext** dla elementu [środowisko uruchomieniowe] również jest **ograniczony**, są używane uprawnienia użytkownika.

- **podwyższonym poziomem uprawnień**  
Zadanie uruchamiania będzie uruchamiane z uprawnieniami administratora. Dzięki temu zadania uruchamiania, aby zainstalować programy, wprowadź zmiany w konfiguracji usług IIS, wykonywanie zmian w rejestrze i inne zadania poziomu administratora bez zwiększania poziom uprawnień roli się.  

> [AZURE.NOTE] Poziom uprawnień uruchamiania zadania nie musi być taka sama jak roli się.

**taskType** — umożliwia określenie sposobu uruchamiania zadania jest wykonywane.

- **prosta**  
Zadania są wykonywane synchronicznie, pojedynczo, w kolejności określonej w pliku [ServiceDefinition.csdef] . Po jednym **prostym** zadaniu uruchamiania kończy się **errorlevel** zero, jest wykonywane następnego **proste** zadania uruchamiania. W przypadku nie więcej **proste** zadania uruchamiania wykonać roli samej zostanie uruchomiony.   

    > [AZURE.NOTE] Jeśli **proste** zadanie kończy się niezerowy **errorlevel**, wystąpieniu zostaną zablokowane. Kolejne **prosta** uruchamiania zadania oraz roli, nie będą uruchamiane.

    Aby upewnić się, że plik wsadowy kończy się **errorlevel** zero, należy wykonać polecenia `EXIT /B 0` na końcu procesu plik wsadowy.

- **tła**  
Zadania wykonywane są asynchroniczne równolegle z uruchamiania roli.

- **aktywne**  
Zadania wykonywane są asynchroniczne równolegle z uruchamiania roli. Kluczowe różnica między **pierwszego planu** i zadania w **tle** to, że zadania **aktywne** uniemożliwia roli z odtwarzanie lub zamykanie aż zadanie zostało zakończone. Nie ma to ograniczenie zadania w **tle** .

## <a name="environment-variables"></a>Zmienne środowiska

Zmienne środowiska służą do przekazania informacji do uruchamiania zadania. Na przykład możesz umieścić ścieżkę blob, w którym znajduje się program, aby zainstalować, numery portów użyje roli użytkownika lub ustawienia, aby funkcje kontroli zadania uruchamiania.

Istnieją dwa rodzaje zmiennych środowiska dla zadań uruchamiania; Zmienne statyczne środowiska i zmienne środowiska w oparciu o członkowie klasy [RoleEnvironment] . Obie znajdują się w sekcji [środowiska] pliku [ServiceDefinition.csdef] , a zarówno za pomocą [zmiennej] atrybutu element oraz **nazwę** .

Zmienne statyczne środowiska używa atrybut **value** elementu [zmiennych] . W powyższym przykładzie powoduje utworzenie zmiennej środowiska **MyVersionNumber** , w której znajduje się wartość statyczną "**1.0.0.0**". Inny przykład może być Utwórz zmienną środowiska **StagingOrProduction** , które można ustawić ręcznie do wartości "**tymczasowej**" lub "**produkcji**" do uruchamiania różne działania na podstawie wartości zmiennej środowiska **StagingOrProduction** .

Zmienne środowiska w oparciu o członkowie klasy RoleEnvironment należy używać atrybut **value** elementu [zmiennych] . Zamiast tego element podrzędny [RoleInstanceValue] odpowiednią wartość atrybutu **XPath** są używane do tworzenia zmiennej środowiska na podstawie określonych składnika klasy [RoleEnvironment] . Wartości atrybutu **XPath** uzyskać dostęp do różnych wartości [RoleEnvironment] można znaleźć [w tym miejscu](cloud-services-role-config-xpath.md).



Na przykład aby utworzyć zmiennej środowiska, który jest "**PRAWDA**", gdy wystąpienie jest uruchomiony w emulatora obliczeń i "**false**", gdy działa w chmurze, należy użyć następujących elementów [zmiennej] i [RoleInstanceValue] :

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak wykonać kilka [typowych zadań uruchamiania](cloud-services-startup-tasks-common.md) za pomocą usługi w chmurze.

[Pakiet](cloud-services-model-and-package.md) usługi w chmurze.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Zadanie]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Uruchamianie]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Środowisko uruchomieniowe]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Środowisko]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Zmienna]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx