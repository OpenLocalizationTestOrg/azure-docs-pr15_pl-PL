<properties
   pageTitle="Konfigurowanie hosta Docker z VirtualBox | Microsoft Azure"
   description="Instrukcje krok po kroku, aby skonfigurować przy użyciu komputera Docker i VirtualBox wystąpienie Docker domyślne"
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
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Konfigurowanie hosta Docker z VirtualBox

## <a name="overview"></a>Omówienie
Ten artykuł prowadzi użytkownika przez skonfigurowanie wystąpienie Docker domyślne przy użyciu komputera Docker i VirtualBox. Jeśli używasz [Docker dla systemu Windows w wersji beta](http://beta.docker.com/)tej konfiguracji nie jest konieczne.

## <a name="prerequisites"></a>Wymagania wstępne
Następujące narzędzia musi być zainstalowany.

- [Zestaw narzędzi docker](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Konfigurowanie klientów Docker przy użyciu programu Windows PowerShell

Aby skonfigurować klienta Docker, po prostu otwórz programu Windows PowerShell i wykonaj następujące czynności:

1. Tworzenie w przypadku wystąpienia domyślnego docker hosta.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Sprawdź, czy wystąpienia domyślnego jest skonfigurowany i działa. (Powinna być widoczna wystąpienie o nazwie "domyślne" uruchomiony.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![wynik ls docker komputera][0]
 
1. Domyślne jako obecnego hosta i konfigurowanie usługi powłoki.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Wyświetlanie aktywnej kontenerów Docker. Lista powinna być pusta.

    ```PowerShell
    docker ps
    ```

    ![dane wyjściowe ps docker][1]
 
> [AZURE.NOTE]Za każdym razem, ponownym uruchomieniu komputera rozwoju, musisz ponownie uruchomić hosta lokalnego docker.
> Aby to zrobić, Wydaj następujące polecenie w wierszu polecenia: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
