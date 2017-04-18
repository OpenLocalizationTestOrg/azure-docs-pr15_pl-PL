<properties
   pageTitle="Przy użyciu rozszerzenia maszyn wirtualnych Docker Azure | Microsoft Azure"
   description="Dowiedz się, jak za pomocą rozszerzenia maszyn wirtualnych Docker szybko i bezpiecznie wdrożyć środowisku Docker platformy Azure korzystania z szablonów Menedżera zasobów."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Tworzenie środowiska Docker w Azure przy użyciu rozszerzenia Docker maszyn wirtualnych
Docker jest zarządzania popularne kontenera i obrazowania platformy, która pozwala na szybkie Praca z kontenerami na Linux (i również w systemie Windows). W Azure istnieją różne sposoby Docker można wdrażać zgodnie z potrzebami. W tym artykule omówiono na temat korzystania z rozszerzeniem Docker maszyn wirtualnych i szablony Azure Menedżera zasobów. 

Aby uzyskać więcej informacji o różnych metodach wdrażania, również za pomocą komputera Docker i usługi Azure kontenera zobacz następujące artykuły:

- Aby szybko prototypu aplikacji można utworzyć jeden host Docker przy użyciu [Komputera Docker](./virtual-machines-linux-docker-machine.md).
- W środowiskach większych, bardziej stabilny umożliwiają rozszerzenie maszyn wirtualnych Docker Azure umożliwia [Redagowanie Docker](https://docs.docker.com/compose/overview/) do generowania wdrożeń spójne kontener. Przy użyciu rozszerzenia maszyn wirtualnych Docker Azure szczegółów tego artykułu.
- Aby utworzyć produkcji gotowe, skalowalna środowiskach, które zapewniają dodatkowe planowania i narzędzi do zarządzania, należy wdrożyć [Docker Swarm klaster na usługi Azure kontener](../container-service/container-service-deployment.md).


## <a name="azure-docker-vm-extension-overview"></a>Omówienie rozszerzenia maszyn wirtualnych Docker Azure
Rozszerzenie maszyn wirtualnych Docker Azure instaluje i konfiguruje demon Docker, Docker klienta i Utwórz Docker na tym komputerze wirtualnych Linux (maszyn wirtualnych). Korzystając z rozszerzeniem maszyn wirtualnych Docker Azure, masz więcej kontroli i funkcji niż po prostu przy użyciu komputera Docker lub tworzenie hosta Docker samodzielnie. Te dodatkowe funkcje, takie jak [Tworzą Docker](https://docs.docker.com/compose/overview/), Ustaw rozszerzenie maszyn wirtualnych Docker Azure, dopasowane do bardziej rozbudowany deweloper lub środowisku produkcyjnym.

Azure szablony Menedżera zasobów zdefiniuj całą strukturę środowiska. Szablony umożliwiają tworzenie i konfigurowanie zasobów, takich jak host Docker maszyny wirtualne, miejsca do magazynowania, kontrola dostępu oparta na rolach (RBAC) i diagnostyki. Można ponownie użyć tych szablonów, aby utworzyć kolejne wdrożenia w sposób ciągły. Aby uzyskać więcej informacji na temat Menedżera zasobów Azure i szablony Zobacz [Omówienie Menedżera zasobów](../azure-resource-manager/resource-group-overview.md). 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Rozmieszczanie szablonu z rozszerzeniem maszyn wirtualnych Docker Azure
Aby utworzyć maszyn wirtualnych Ubuntu z rozszerzeniem maszyn wirtualnych Docker Azure służy do instalowania i konfigurowania hosta Docker, można użyć istniejącego szablonu Szybki Start. Można wyświetlić szablonu tutaj: [proste wdrożenie maszyn wirtualnych Ubuntu z Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Potrzebujesz [Najnowszych polecenie Azure](../xplat-cli-install.md) zainstalowane i zarejestrowane w trybie Menedżera zasobów w następujący sposób:

```
azure config mode arm
```

Wdrażanie szablonu, za pomocą polecenie Azure, określając szablon identyfikatora URI. Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `WestUS` lokalizacji. Używanie własnych Nazwa grupy zasobów i lokalizacji:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Odpowiedzi z monitami, aby nazwę konta magazynu, podanie nazwy użytkownika i hasła i podaj nazwę DNS. Wynik jest podobny do następującego przykładu:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Zwracane przez polecenie Azure monit po zaledwie kilku sekund, ale hosta Docker nadal utworzony i skonfigurowany przez rozszerzenie maszyn wirtualnych Docker Azure. Wystarczy kilka minut do wdrożenia zakończyć. Można wyświetlać informacje na temat używania stan hosta Docker `azure vm show` polecenia.

W poniższym przykładzie sprawdzana stanu maszyn wirtualnych, o nazwie `myDockerVM` (domyślna nazwa w szablonie — nie zmienić tę nazwę) w grupie zasobów o nazwie `myResourceGroup`. Wprowadź nazwę grupy zasobów, który został utworzony w poprzednim kroku:

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

Wynik `azure vm show` polecenie jest podobny do następującego przykładu:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

W górnej części wyniku, zobacz `ProvisioningState` z maszyn wirtualnych. Gdy zostanie wyświetlony `Succeeded`, wdrażanie zostało zakończone i czy SSH do maszyn wirtualnych.

Pod koniec dane wyjściowe `FQDN` wyświetla w pełni kwalifikowaną nazwę domeny swojego hosta Docker. Tej nazwy FQDN to, co umożliwia SSH do hosta Docker w pozostałe kroki.


## <a name="deploy-your-first-nginx-container"></a>Wdrażanie usługi pierwszym kontenerze nginx
Po Wdrażanie zostało zakończone, SSH do nowego hosta Docker z komputera lokalnego. Wprowadź własną nazwę użytkownika i FQDN w następujący sposób:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Po zalogowaniu się hosta Docker, Przejdźmy Uruchom kontenerem nginx:

```
sudo docker run -d -p 80:80 nginx
```

Wynik jest podobny do następującego przykładu jako obraz nginx jest pobierany i kontenera pracę:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Sprawdzanie stanu kontenerów uruchomionych hosta Docker w następujący sposób:

```
sudo docker ps
```

Wynik jest podobny do następującego przykładu, wskazujący, że kontener nginx jest uruchomiona, a porty TCP 80 i 443 i przesyłane dalej:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Aby wyświetlić do kontenera w działaniu, otwórz przeglądarkę sieci web i wprowadź nazwę FQDN hosta Docker:

![Bieżąca kontener ngnix](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Odwołanie do szablonu rozszerzenia Azure maszyn wirtualnych Docker
W poprzednim przykładzie użyto istniejącego szablonu Szybki Start. Można także wdrożyć rozszerzenia maszyn wirtualnych Docker Azure z szablonów Menedżera zasobów. Aby to zrobić, Dodaj następujący do szablonów Menedżera zasobów, definiując `vmName` z usługi maszyn wirtualnych odpowiednio:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Można znaleźć bardziej szczegółowe instrukcje na temat korzystania z szablonów Menedżera zasobów, czytając [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md).


## <a name="next-steps"></a>Następne kroki
Możesz [skonfigurować demona Docker TCP port](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), zapoznanie się z [zabezpieczeniami Docker](https://docs.docker.com/engine/security/security/)lub wdrażanie kontenerów przy użyciu [Docker redagowanie](https://docs.docker.com/compose/overview/). Aby uzyskać więcej informacji o rozszerzeniu maszyn wirtualnych Docker Azure samej zobacz [GitHub projektu](https://github.com/Azure/azure-docker-extension/).

Przeczytaj więcej informacji na temat dodatkowe opcje wdrażania Docker platformy Azure:

- [Za pomocą sterownika Azure za pomocą komputera Docker](./virtual-machines-linux-docker-machine.md)  
- [Rozpoczynanie pracy z Docker i redagowanie zdefiniować i uruchom aplikację kontenera wielu elementów na Azure maszyn wirtualnych](virtual-machines-linux-docker-compose-quickstart.md).
- [Wdrażanie klastrze Usługa Azure kontenera](../container-service/container-service-deployment.md)
