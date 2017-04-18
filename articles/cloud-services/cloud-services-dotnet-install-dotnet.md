<properties
   pageTitle="Instalowanie programu .NET na rolę usługi Cloud | Microsoft Azure"
   description="W tym artykule opisano, jak ręcznie zainstalować program .NET framework na sieci Web usługi w chmurze i ról pracownika"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>Instalowanie programu .NET na rolę usługi chmury 

W tym artykule opisano, jak zainstalować program .NET framework na sieci Web usługi w chmurze i ról pracownika. Te kroki umożliwia instalowanie .NET 4.6.1 na 4 Rodzina systemu operacyjnego gościa Azure. Aby uzyskać najnowsze informacje o wersjach systemu operacyjnego gościa zobacz [Azure gościa, za pomocą systemu operacyjnego i SDK zgodności macierzy](cloud-services-guestos-update-matrix.md).

Proces instalacji .NET na poszczególnych ról w sieci web i pracowników polega na tym pakiet Instalatora .NET w ramach projektu chmury i uruchamiania Instalatora pakietu jako część tej roli uruchamiania zadań.  

## <a name="add-the-net-installer-to-your-project"></a>Dodawanie Instalatora .NET do projektu
- Pobierz Instalatora sieci web dla programu .NET framework, którą chcesz zainstalować
    - [.NET 4.6.1 web Instalatora](http://go.microsoft.com/fwlink/?LinkId=671729)
- Dla ról w sieci Web
  1. W **Eksploratorze rozwiązań**w obszarze **role** w projekcie chmury usługi kliknij prawym przyciskiem myszy rolę i wybierz pozycję **Dodaj > Nowy Folder**. Utwórz folder o nazwie *Kosza*
  2. Kliknij prawym przyciskiem myszy **bin** folder i wybierz **Dodaj > istniejący element**. Wybierz Instalatora .NET i dodaj go do folderu bin.
- Dla roli pracownika
  1. Kliknij prawym przyciskiem myszy rolę i wybierz pozycję **Dodaj > istniejący element**. Wybierz Instalatora .NET i dodaj go do roli. 

Pliki dodane do folderu zawartości roli dzięki temu zostaną automatycznie dodane do pakietu z chmury i wdrożony spójne lokalizację na komputerze wirtualnych. Powtórz tę procedurę dla wszystkich ról w sieci web i pracowników w usłudze w chmurze, więc wszystkich ról kopię Instalatora.

> [AZURE.NOTE] Należy zainstalować .NET 4.6.1 na swojej roli usługi w chmurze nawet wtedy, gdy aplikacja jest przeznaczony dla .NET 4.6. System operacyjny gościa Azure obejmuje aktualizacje [3098779](https://support.microsoft.com/kb/3098779) i [3097997](https://support.microsoft.com/kb/3097997). Instalowanie .NET 4.6 u góry te aktualizacje może powodować problemy podczas uruchamiania aplikacji .NET, należy zainstalować bezpośrednio .NET 4.6.1 zamiast .NET 4.6. Aby uzyskać więcej informacji zobacz [KB 3118750](https://support.microsoft.com/kb/3118750).

![Zawartość roli z plikami Instalatora][1]

## <a name="define-startup-tasks-for-your-roles"></a>Definiowanie uruchamiania zadań dla poszczególnych ról
Uruchamianie zadania umożliwiają wykonywanie operacji przed uruchomieniem roli. Instalowanie programu .NET Framework jako części zadania uruchamiania zapewni zainstalowania ramach, zanim dowolnego kodu aplikacji jest uruchamiany. Aby uzyskać więcej informacji na temat uruchamiania Zobacz zadania: [Uruchamianie zadania uruchamiania w Azure](cloud-services-startup-tasks.md). 

1. Dodaj następujący do pliku *ServiceDefinition.csdef* węźle **WebRole** lub **WorkerRole** dla wszystkich ról:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    Konfiguracja powyżej spowoduje uruchomienie polecenia konsoli *install.cmd* z uprawnieniami administratora, więc można zainstalować programu .NET framework. Konfiguracja tworzy również LocalStorage o nazwie *NETFXInstall*. Skrypt uruchamiania zostanie ustawiona folderu temp używania tego zasobu magazynu lokalnego, tak aby Instalatora programu .NET framework zostaną pobrane i zainstalowane z tego zasobu. Należy ustawić rozmiar tego zasobu na co najmniej 1024MB, aby upewnić się, że w ramach zostaną zainstalowane poprawnie. Aby uzyskać więcej informacji na temat uruchamiania Zobacz zadania: [usługi w chmurze typowych zadań uruchamiania](cloud-services-startup-tasks-common.md) 

2. Tworzenie pliku **install.cmd** je dodać do wszystkich ról, kliknij prawym przyciskiem roli i wybierając **Dodaj > istniejący element**. Aby wszystkie role ma teraz plik Instalatora .NET, a także plik install.cmd.
    
    ![Zawartość roli z wszystkimi plikami][2]

    > [AZURE.NOTE] Aby utworzyć ten plik za pomocą edytora tekstu zwykłego, takich jak Notatnik. Jeśli korzystasz z programu Visual Studio do tworzenia pliku tekstowego i zmień jej nazwę na "cmd" plik nadal może zawierać znak porządku bajtów UTF-8 i uruchomiony pierwszy wiersz skrypt spowoduje komunikat o błędzie. W przypadku programu Visual Studio umożliwia tworzenie Opuść pliku dodać REM (Uwaga) do pierwszego wiersza pliku, tak aby jest ignorowana, podczas uruchamiania. 

3. Dodaj następujący skrypt do pliku **install.cmd** :

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Skrypt instalacji sprawdza, czy określonej wersji programu .NET framework jest już zainstalowany na komputerze za pomocą kwerend wysyłanych rejestru. Jeśli nie zainstalowano programu .NET w wersji program .net Instalatora sieci Web jest uruchomiona. Aby ułatwić rozwiązywanie problemów z jakichkolwiek problemów z skrypt można rejestrować wszystkie operacje w pliku o nazwie *startuptasklog-(currentdatetime) txt* przechowywane w magazynie lokalnym *InstallLogs* .

    > [AZURE.NOTE] Skrypt nadal pokazano, jak zainstalować program .NET 4.5.2 lub .NET 4.6 ciągłość. Istnieje nie trzeba ręcznie zainstalować program .NET 4.5.2 jest już dostępna w systemie OS gościa Azure. Zamiast instalowania .NET 4.6 bezpośrednio powinni zainstalować .NET 4.6.1 z powodu [KB 3118750](https://support.microsoft.com/kb/3118750).
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Konfigurowanie diagnostyki przesyłanie dzienniki uruchamiania zadań w celu blob miejsca do magazynowania 
W celu uproszczenia, rozwiązywanie problemów z jakichkolwiek problemów z instalacji można skonfigurować diagnostyki Azure, aby przenieść wszystkie pliki dziennika generowane przez skrypt uruchamiania lub Instalatora .NET magazynem obiektów blob. Z tej metody można wyświetlić dzienniki, po prostu pobieranie plików dziennika z magazynem obiektów blob zamiast pulpitu zdalnego do roli.

Aby skonfigurować Otwórz diagnostyki *diagnostics.wadcfgx* i Dodaj następujący węźle **katalogów** : 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Skonfiguruj azure diagnostyki przesyłanie wszystkie pliki w katalogu *dziennika* w obszarze zasobów *NETFXInstall* diagnostyki konta miejsca do magazynowania w kontenerze obiektów blob *netfx instalacji* .

## <a name="deploying-your-service"></a>Wdrażanie usługi 
Podczas wdrażania usługi zadania uruchamiania uruchom i instalowanie programu .NET framework, jeśli nie jest już zainstalowany. Poszczególnych ról będzie w stanie zajęty podczas ramach instaluje i może nawet jeśli ponowne uruchomienie wymaga instalacji framework. 

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Instalowanie programu .NET Framework][]
- [Jak: Ustalanie, które .NET Framework wersje są zainstalowane][]
- [Rozwiązywanie problemów z .NET Framework instalacji][]

[Jak: Ustalanie, które .NET Framework wersje są zainstalowane]: https://msdn.microsoft.com/library/hh925568.aspx
[Instalowanie programu .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Rozwiązywanie problemów z .NET Framework instalacji]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
