<properties
   pageTitle="Wdrażanie kontenerem ASP.NET Core Linux Docker się z hostem zdalnym Docker | Microsoft Azure"
   description="Dowiedz się, jak za pomocą programu Visual Studio Tools for Docker wdrażanie aplikacji sieci web programu ASP.NET Core kontenera Docker uruchomionych maszyn wirtualnych Linux Azure Docker hosta"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Wdrażanie kontenerem ASP.NET z hostem zdalnym Docker

## <a name="overview"></a>Omówienie
Docker jest aparatem lightweight kontenera, podobne na kilka sposobów maszyn wirtualnych, które służy do obsługi aplikacji i usług.
Ten samouczek przeprowadzi Cię przez za pomocą rozszerzenia [Visual Studio 2015 Tools Docker](http://aka.ms/DockerToolsForVS) wdrażanie aplikacji ASP.NET Core z hostem Docker Azure przy użyciu programu PowerShell.

## <a name="prerequisites"></a>Wymagania wstępne
Następujące czynności są wymagane do ukończenia tego samouczka:

- Tworzenie maszyn wirtualnych Azure Docker hosta, zgodnie z opisem w [sposób używania docker maszyny liczącej z platformy Azure](./virtual-machines/virtual-machines-linux-docker-machine.md)
- Aktualizacji [programu Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Zestaw SDK programu Microsoft ASP.NET Core 1.0](https://go.microsoft.com/fwlink/?LinkID=809122)
- Instalowanie [Visual Studio 2015 Tools for Docker - Podgląd](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Tworzenie aplikacji sieci web programu ASP.NET Core
Poniższe kroki przeprowadzi Cię przez proces tworzenia podstawowe aplikacji podstawowych ASP.NET, która będzie używana w tym samouczku.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Dodaj Docker pomocy technicznej

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. skorzystaj z tego skryptu programu PowerShell DockerTask.ps1 

1.  Otwórz wiersz programu PowerShell do katalogu głównego projektu. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Sprawdź poprawność zdalnego host jest uruchomiony. Powinien zostać wyświetlony stan = pracy 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Jeśli używasz wersji Beta Docker hosta nie będą tu wymienione.

1.  Tworzenie aplikacji przy użyciu parametru - kompilacji

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Jeśli korzystasz z wersji Beta Docker, pominąć argument - komputera
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Uruchamianie aplikacji, przy użyciu parametru - Uruchom

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Jeśli korzystasz z wersji Beta Docker, pominąć argument - komputera
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Po ukończeniu działania docker powinny zostać wyświetlone wyniki podobne do następujących czynności:

    ![Wyświetlanie aplikacji][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
