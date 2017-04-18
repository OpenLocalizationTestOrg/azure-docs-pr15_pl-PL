<properties
   pageTitle="Rozwiązywanie problemów z błędami klienta Docker w systemie Windows przy użyciu programu Visual Studio | Microsoft Azure"
   description="Rozwiązywanie problemów napotkanych podczas używania programu Visual Studio do tworzenie i wdrażanie aplikacji sieci web przy użyciu programu Visual Studio Docker w systemie Windows."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Rozwiązywanie problemów z rozwoju Docker programu Visual Studio

Podczas pracy z Visual Studio Tools dla Docker Preview, mogą wystąpić problemy ze względu na charakter Podgląd.
Poniżej przedstawiono kilka typowych problemów i rozwiązań.


## <a name="unable-to-validate-volume-mapping"></a>Nie można sprawdzić poprawności mapowania głośności
Mapowanie głośność jest wymagane do udostępniania kodu źródłowego i plików binarnych aplikacji z folderem aplikacji w kontenerze.  Mapowania określonego woluminu są zawarte w pliku docker compose.dev.debug.yml i docker compose.dev.release.yml. Jak pliki zostaną zmienione na komputerze hosta, kontenerów odzwierciedlają te zmiany w strukturze podobne folderu.

Aby włączyć mapowanie głośność, należy otworzyć okno **Ustawienia...** za pomocą ikony obszar powiadomień "moby" Docker dla systemu Windows, a następnie wybierz kartę **Udostępnionych dysków** .  Upewnij się, są udostępniane literę, który obsługuje projektu, a także literę, w którym znajduje się % USERPROFILE %, sprawdzanie ich, a następnie klikając polecenie **Zastosuj**.

Aby sprawdzić, czy mapowanie głośność działa, gdy dyski zostały udostępnione, Odbuduj i F5 z poziomu programu Visual Studio lub spróbuj następujące polecenia Monituj:

*W wierszu polecenia systemu Windows*

*[Uwaga: to zakłada folderu użytkownicy znajduje się na dysku "C" i został udostępniony.  Aktualizowanie stosownie do potrzeb, jeśli masz udostępniony innego dysku]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*W kontenerze Linux*

```
/ # ls
```

Powinien zostać wyświetlony katalogu z folderu publicznego i użytkowników.
Jeśli są wyświetlane żadne pliki i folderu /c/Users/Public nie jest puste, mapowanie głośności nie skonfigurowano poprawnie. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Przekształcanie katalog tunel, aby wyświetlić zawartość `/c/Users/Public` katalogu:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Uwaga:** *Podczas pracy z maszyny wirtualne Linux, system plików kontener jest uwzględniana wielkość liter.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Kompilacja: "PrepareForBuild" niepowodzenie zadania.

Microsoft.DotNet.Docker.CommandLine.ClientException: Wystąpił błąd podczas nawiązywania połączenia:

Sprawdź, czy jest uruchomiony host docker domyślne. Otwórz okno wiersza polecenia i wykonać:

```
docker info
```

Jeśli to zwraca błąd spróbuj uruchomić aplikację klasyczną **Docker dla systemu Windows** .  Jeśli aplikacja klasyczna działa następnie **moby** ikona na pasku zadań powinna być widoczna. Kliknij prawym przyciskiem myszy ikonę w obszarze powiadomień, a następnie otwórz **Ustawienia**.  Polecenie **Resetuj** kartę, a następnie **Uruchom ponownie Docker..**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Ręczne uaktualnianie wersji 0.31 do 0,40


1. Aby utworzyć kopię zapasową projektu
1. Usuń następujące pliki w projekcie:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Zamknij rozwiązanie i Usuń następujące wiersze z pliku .xproj:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Otwórz ponownie rozwiązanie
1. Usuń następujące wiersze z pliku Properties\launchSettings.json:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Usuń następujące pliki związane z project.json w publishOptions Docker:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Odinstaluj poprzednią wersję i zainstaluj narzędzia Docker 0,40, a następnie **Dodaj -> Obsługa Docker** ponownie z menu kontekstowego dla sieci Web programu ASP.Net podstawowego lub aplikacji konsoli. Spowoduje to dodanie nowego wymagane artefakty Docker powrót do projektu. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Okno dialogowe występuje w momencie próby **Dodaj -> Obsługa Docker** lub debugowania (F5) aplikacji podstawowych ASP.NET w kontenerze

Firma Microsoft od czasu do czasu przejrzane odinstalowaniu i instalowanie rozszerzeń, pamięci podręcznej MEF (zarządzanych rozszerzeń struktura) programu Visual Studio może zostać uszkodzony. Gdy dzieje się tak że może spowodować błąd różne okna podczas dodawania Docker pomocy technicznej i/lub próby uruchomienia lub debugowanie aplikacji podstawowych ASP.NET (F5). Tymczasowe obejść ten problem wykonaj następujące czynności, aby usunąć i ponownie wygenerować MEF pamięci podręcznej.

1. Zamknij wszystkie wystąpienia programu Visual Studio
1. Otwórz %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\
1. Usuń następujące foldery
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Otwieranie programu Visual Studio
1. Ponowne wykonanie tego scenariusza 
