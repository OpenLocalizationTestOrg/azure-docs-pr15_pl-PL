<properties
   pageTitle="Tworzenie Docker hosts w Azure z komputerem Docker | Microsoft Azure"
   description="W tym artykule opisano używanie komputera Docker tworzenie docker hosts w Azure."
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

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Tworzenie Docker Hosts w Azure z komputerem Docker

Uruchamianie kontenerów [Docker](https://www.docker.com/) wymaga hosta maszyn wirtualnych uruchamiania demona docker.
W tym temacie opisano, jak utworzyć nowy Linux maszyny wirtualne, skonfigurowano demona Docker uruchomione w Azure za pomocą polecenia [docker komputera](https://docs.docker.com/machine/) . 

**Uwaga:** 
- *W tym artykule zależy od komputera docker wersji 0.7.0 lub nowszej*
- *Kontenery Windows będą obsługiwane przez komputer docker w najbliższej przyszłości*

## <a name="create-vms-with-docker-machine"></a>Tworzenie maszyny wirtualne z komputerem Docker

Tworzenie docker maszyny wirtualne hosta w Azure z `docker-machine create` polecenia `azure` sterownika. 

Sterownik Azure muszą swojego identyfikatora subskrypcji. [Polecenie Azure](xplat-cli-install.md) lub [Azure Portal](https://portal.azure.com) umożliwia pobieranie subskrypcji usługi Azure. 

**Za pomocą portalu Azure**
- Zaznacz subskrypcji na stronie nawigacji po lewej stronie, a następnie skopiuj identyfikator subskrypcji.

**Za pomocą polecenie Azure**
- Typ ```azure account list``` i skopiuj identyfikator subskrypcji.

Typ `docker-machine create --driver azure` Aby wyświetlić opcje i ich wartości domyślne.
Można też wyświetlić w [dokumentacji sterownika Azure Docker](https://docs.docker.com/machine/drivers/azure/) uzyskać więcej informacji. 

Poniższy przykład opiera się na wartości domyślne, ale opcjonalnie go otworzyć portu 80 maszyn wirtualnych dostęp do Internetu. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Wybierz pozycję host docker z komputerem docker
Po umieszczeniu wpis na komputerze docker dla hosta podczas uruchamiania polecenia docker można ustawić host domyślny.
##<a name="using-powershell"></a>Przy użyciu programu PowerShell

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Za pomocą imprezie

```bash
eval $(docker-machine env MyDockerHost)
```

Teraz możesz uruchomić polecenia docker przed wskazanego hosta

```
docker ps
docker info
```

## <a name="run-a-container"></a>Uruchamianie kontenera

Skonfigurowanego umożliwia teraz uruchamianie serwera sieci web proste, aby sprawdzić, czy hosta został poprawnie skonfigurowany.
Tutaj wyświetlonym firma Microsoft za pomocą obrazu nginx standardowy, określić, że należy odsłuchać na porcie 80 i że po ponownym uruchomieniu hosta maszyn wirtualnych kontenerze będą ponownie uruchomić także (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Wynik powinien wyglądać następująco:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testowanie kontenera

Sprawdzenie kontenerów uruchomionego za pomocą `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

I sprawdź uruchomionego kontener, wpisz `docker-machine ip <VM name>` znaleźć adres IP, aby wprowadzić w przeglądarce:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Bieżąca kontener ngnix](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Podsumowanie
Z komputerem docker umożliwia łatwe obsługę hosts docker platformy Azure dla swojego hosta poszczególnych docker, reguły sprawdzania poprawności.
Do obsługi produkcji kontenerów, zobacz [Usługa Azure kontenera](http://aka.ms/AzureContainerService)

Aby przygotować .NET podstawowych aplikacji przy użyciu programu Visual Studio, zobacz [Narzędzia Docker programu Visual Studio](http://aka.ms/DockerToolsForVS)
