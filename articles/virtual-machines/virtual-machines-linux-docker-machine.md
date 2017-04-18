<properties
    pageTitle="Tworzenie Docker hosts w Azure z komputerem Docker | Microsoft Azure"
    description="W tym artykule opisano używanie komputera Docker tworzenie docker hosts w Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Za pomocą sterownika Azure za pomocą komputera Docker

[Docker](https://www.docker.com/) jest jedną z najpopularniejszych metod wirtualizacji używanych kontenerów Linux zamiast w środowisku maszyn wirtualnych systemu sposób izolowanie dane aplikacji i obliczania do zasobów udostępnionych. W tym temacie opisano, kiedy i jak używać [Komputera Docker](https://docs.docker.com/machine/) ( `docker-machine` polecenie) do tworzenia nowych Linux maszyny wirtualne w Azure włączone jako host docker dla kontenerów Linux.


## <a name="create-vms-with-docker-machine"></a>Tworzenie maszyny wirtualne z komputerem Docker

Tworzenie docker maszyny wirtualne hosta w Azure z `docker-machine create` polecenia `azure` argument sterownik dla opcji sterownik (`-d`) oraz innych argumentów. 

Poniższy przykład opiera się na wartości domyślne, ale otwieranie portu 80 maszyn wirtualnych z Internetem, aby przetestować z kontenerem nginx, powoduje, że `ops` zalogowany użytkownik SSH i połączeń nowa maszyna wirtualna `machine`. 

Typ `docker-machine create --driver azure` Aby wyświetlić opcje i ich wartościami domyślnymi; można również przeczytać w [dokumentacji sterownika Azure Docker](https://docs.docker.com/machine/drivers/azure/). (Należy zauważyć, że jeśli włączono uwierzytelnianie dwuskładnikowe, pojawi się monit do uwierzytelnienia przy użyciu współczynnika drugiego).

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

Wynik powinna wyglądać podobnie, zależnie od tego, czy masz uwierzytelnianie dwuskładnikowe skonfigurowane na Twoim koncie.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>Konfigurowanie usługi powłoki docker

Teraz, wpisz `docker-machine env <VM name>` Aby sprawdzić, co należy zrobić, aby skonfigurować powłokę. 

```bash
docker-machine env machine
```

Który wyświetla informacje o środowisku, który wygląda mniej więcej tak. Należy zauważyć, że adres IP został przypisany, które musisz przetestować maszyn wirtualnych.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Można uruchomić polecenia sugerowane konfiguracji, lub możesz zmienne środowiska można ustawić. 

## <a name="run-a-container"></a>Uruchamianie kontenera

Teraz można uruchamiać serwera sieci web proste, aby sprawdzić czy wszystko działa poprawnie. W tym miejscu wyświetlonym firma Microsoft za pomocą obrazu nginx standardowy, określić, że należy odsłuchać na porcie 80 i że jeśli ponowne uruchomienie maszyn wirtualnych kontenerze należy ponownie uruchomić także (`--restart=always`). 

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

I sprawdź uruchomionego kontener, wpisz `docker-machine ip <VM name>` znaleźć adres IP (Jeśli nie pamiętasz z `env` polecenia):

![Bieżąca kontener ngnix](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz, możesz wypróbować Azure [Docker maszyn wirtualnych rozszerzenia](virtual-machines-linux-dockerextension.md) do tej samej operacji za pomocą interfejsu wiersza polecenia Azure lub szablony Menedżera zasobów Azure. 

Więcej przykładów pracy z Docker zobacz [Praca z Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) z Połącz [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [Pokaz](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Aby uzyskać więcej Przewodniki Szybki Start z pokaz HealthClinic.biz zobacz [Azure Deweloper narzędzia Przewodniki Szybki Start](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

