<properties
    pageTitle="Role witryny sieci web i pracownik Python z programem Visual Studio | Microsoft Azure"
    description="Omówienie tworzenie usług w chmurze Azure tym ról w sieci web i pracowników za pomocą narzędzia Python programu Visual Studio."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Role witryny sieci web i pracownik Python narzędziami Python programu Visual Studio

Ten artykuł zawiera omówienie korzystania z ról w sieci web i pracownik Python za pomocą [Narzędzi Python programu Visual Studio][]. Będzie się, jak za pomocą programu Visual Studio tworzyć i wdrażać podstawowe usługi w chmurze, która używa Python.

## <a name="prerequisites"></a>Wymagania wstępne

 - Visual Studio 2013 lub 2015 r.
 - [Narzędzia Python programu Visual Studio][] (PTVS)
 - [Narzędzia Azure SDK 2013 w PORÓWNANIU z][] lub [Azure SDK w PORÓWNANIU z 2015 r.][]
 - [Python 2.7 32-bitowej][] lub [Python 3.5 32-bitowej][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Co to są role sieci web i pracownik Python?

Azure oferuje trzy obliczyć modeli uruchamiania aplikacji: [Funkcja aplikacji sieci Web w usłudze Azure aplikacji][execution model-web sites], [maszyn wirtualnych Azure][execution model-vms]i [Usług w chmurze Azure][execution model-cloud services]. Wszystkich trzech modeli obsługuje Python. Usługami w chmurze, obejmujące ról w sieci web i pracownik stanowią *platformę usług (PaaS)*. W ramach usługi w chmurze dedykowane serwer sieci web Internetowe usługi informacyjne (IIS) do obsługi aplikacji frontonu sieci web, zapewnia ról w sieci web, gdy roli Pracownik może zostać uruchomiony asynchroniczne, długim lub bezterminowo następujące zadania niezależnie od interakcji użytkownika lub danych wejściowych.

Aby uzyskać więcej informacji, zobacz [Co to jest usługa w chmurze?].

> [AZURE.NOTE]*Looking do tworzenia prostych witryny sieci Web?*
Jeśli rozwiązania obejmuje tylko prosta witryny sieci Web frontonu, warto rozważyć użycie najprostsze funkcji aplikacji sieci Web w usłudze Azure aplikacji. Można łatwo uaktualnić do usługi w chmurze rozwoju witryny sieci Web i zmienianie z własnymi potrzebami. Odwiedź witrynę <a href="/develop/python/">Centrum deweloperów Python</a> artykułów, które obejmują opracowywania funkcji aplikacji sieci Web w usłudze Azure aplikacji.
<br />


## <a name="project-creation"></a>Tworzenie projektu

W programie Visual Studio można wybrać **Usługi w chmurze Azure** w oknie dialogowym **Nowy projekt** w obszarze **Python**.

![Okno dialogowe nowego projektu](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

W Kreatorze usługi w chmurze Azure można utworzyć nowe role sieci web i pracownika.

![Okno dialogowe usługi Azure chmury](./media/cloud-services-python-ptvs/new-service-wizard.png)

Szablon roli Pracownik zawiera kod standardowy, aby nawiązać połączenie z kontem Azure miejsca do magazynowania lub Bus usługi Azure.

![Rozwiązania usługi w chmurze](./media/cloud-services-python-ptvs/worker.png)

Role sieci web lub pracownika można dodać do istniejącego usługi w chmurze w dowolnym momencie.  Możesz dodać istniejących projektów w rozwiązaniu ani tworzyć nowych plików.

![Dodawanie polecenia w roli](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Usługa w chmurze może zawierać role w różnych językach.  Na przykład możesz mieć rolę web Python zaimplementować za pomocą Django, Python lub C# role pracownika.  Możesz łatwo komunikować się między poszczególnych ról przy użyciu usługi Bus kolejki lub kolejki miejsca do magazynowania.

## <a name="install-python-on-the-cloud-service"></a>Instalowanie Python na usługi w chmurze

>[AZURE.WARNING] Nie działają skrypty konfiguracji, które są instalowane z programu Visual Studio (w czasie, w tym artykule ostatniej aktualizacji). W tej sekcji opisano obejść ten problem.

Główny problem z skryptów instalacji to, że nie instaluj python. Najpierw należy zdefiniować dwa [zadania uruchamiania](cloud-services-startup-tasks.md) w pliku [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) . Pierwszego zadania (**PrepPython.ps1**) pobiera i instaluje środowisko uruchomieniowe Python. Drugim zadaniem (**PipInstaller.ps1**) jest uruchamiany pip, aby zainstalować wszelkie zależności, który może być.

Poniższe skrypty napisane kierowanie Python 3.5. Jeśli chcesz używać wersji 2.x python, Ustaw plik zmiennych **PYTHON2** **na** dwóch zadań uruchamiania i zadanie w czasie wykonywania: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

Zmienne **PYTHON2** i **PYPATH** musi zostać dodany do zadania uruchamiania pracownika. Zmienna **PYPATH** jest używana tylko, jeśli zmienna **PYTHON2** jest ustawiona **na**.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Przykładowe ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Następnie należy utworzyć pliki **PrepPython.ps1** i **PipInstaller.ps1** w **. / bin** folder roli użytkownika.

#### <a name="preppythonps1"></a>PrepPython.ps1

Ten skrypt instaluje python. Jeśli zmienna enviornment **PYTHON2** jest ustawiona **na** , a następnie Python 2.7 zostanie zainstalowana, w przeciwnym razie zostanie zainstalowana Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

Ten skrypt nawiąże połączenie w górę pip i instaluje wszystkie zależności w pliku **requirements.txt** . Jeśli zmienna enviornment **PYTHON2** jest ustawiona **na** , a następnie Python 2.7 będzie używana, w przeciwnym razie zostanie użyty Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>Modyfikowanie LaunchWorker.ps1

>[AZURE.NOTE] W przypadku projektu **roli Pracownik** plik **LauncherWorker.ps1** jest wymagany do wykonania w pliku startowym. W projekcie **ról w sieci web** plik uruchamiania zamiast tego jest zdefiniowane w właściwości projektu.

**Bin\LaunchWorker.ps1** został pierwotnie utworzony do częstego przygotowań, ale naprawdę nie działa. Zamień zawartość w tym pliku następujący skrypt.

Ten skrypt wywołuje plik **worker.py** z Twojego projektu python. Jeśli zmienna enviornment **PYTHON2** jest ustawiona **na** , a następnie Python 2.7 będzie używana, w przeciwnym razie zostanie użyty Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

Szablony programu Visual Studio należy utworzony plik **ps.cmd** w **. / bin** folder. Skrypt powłoki wymaga się skryptów programu PowerShell opakowanie powyżej i zapewnia rejestrowanie na podstawie nazwy opakowanie programu PowerShell o nazwie. Jeśli ten plik nie został utworzony, Oto, co należy w nim. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Uruchom lokalnie

Ustawianie projektu usługi cloud jako projekt startowy i naciśnij klawisz F5, usługa w chmurze będzie działać w lokalnym emulatora Azure.

Mimo że PTVS obsługuje uruchamianie w emulatorze, debugowania (na przykład punkty) nie będzie działać.

Debugowanie role usługi sieci web i pracownika, można ustawić projektu roli jako projekt startowy i debugowanie który zamiast tego.  Można także ustawić wiele projektów uruchamiania.  Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Ustaw projektów uruchamiania**.

![Właściwości projektu uruchamiania rozwiązanie](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Publikowanie Azure

Aby opublikować, kliknij prawym przyciskiem myszy projektu usługi cloud w rozwiązanie, a następnie wybierz pozycję **Publikuj**.

![Zaloguj się publikowanie platformy Microsoft Azure](./media/cloud-services-python-ptvs/publish-sign-in.png)

Wykonaj instrukcje kreatora. Jeśli potrzebujesz, należy włączyć Pulpit zdalny. Pulpit zdalny jest przydatne, gdy trzeba coś debugowanie.

Po zakończeniu konfigurowania ustawień kliknij przycisk **Publikuj**.

Niektóre informacje o postępie będą wyświetlane w oknie dane wyjściowe, a następnie pojawi się okno Microsoft Azure dziennik.

![Okno Dziennik działań platformy Microsoft Azure](./media/cloud-services-python-ptvs/publish-activity-log.png)

Wdrożenie może potrwać kilka minut do wykonania, a następnie role usługi sieci web i/lub pracownika będą działać na Azure!

### <a name="investigate-logs"></a>Badanie dzienników

Po maszyny wirtualnej chmury usługi uruchamiania i instaluje Python, może przeglądać dzienniki, aby znaleźć wszelkie komunikaty o błędach. Te dzienniki znajdują się w **C:\Resources\Directory\\\LogFiles {roli}** folder. **PrepPython.err.txt** będą mieć co najmniej jeden błąd w nim z, gdy skrypt próbuje wykryć, jeśli zainstalowano Python i **PipInstaller.err.txt** może być skarżyć nieaktualna wersja pip.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać bardziej szczegółowe informacje na temat pracy z sieci web i pracownik ról w obszarze narzędzia Python programu Visual Studio zapoznaj się z dokumentacją PTVS:

- [Projekty usługi w chmurze][]

Aby uzyskać więcej informacji o korzystaniu z usługi Azure z usługi sieci web i pracownik role, takie jak za pomocą magazyn Azure lub usługi Bus zobacz następujące artykuły.

- [Usługa blob][]
- [Usługa tabeli][]
- [Usługa kolejki][]
- [Usługa Bus kolejki][]
- [Usługa Bus tematów][]


<!--Link references-->

[Co to jest usługa w chmurze?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[Usługa obiektów blob]: ../storage/storage-python-how-to-use-blob-storage.md
[Usługa kolejki]: ../storage/storage-python-how-to-use-queue-storage.md
[Usługa tabeli]: ../storage/storage-python-how-to-use-table-storage.md
[Usługa Bus kolejki]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Usługa Bus tematów]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Narzędzia Python programu Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Projekty usługi w chmurze]: http://go.microsoft.com/fwlink/?LinkId=624028
[Narzędzia Azure SDK 2013 w PORÓWNANIU z]: http://go.microsoft.com/fwlink/?LinkId=323510
[Narzędzia Azure SDK w PORÓWNANIU z 2015 r.]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bitowej]: https://www.python.org/downloads/
[Python 3.5 32-bitowej]: https://www.python.org/downloads/
