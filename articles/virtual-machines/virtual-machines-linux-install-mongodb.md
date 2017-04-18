<properties
   pageTitle="Instalowanie MongoDB na maszyny Linux | Microsoft Azure"
   description="Dowiedz się, jak zainstalować i skonfigurować MongoDB na komputerze wirtualnych Linux platformy Azure przy użyciu modelu wdrożenia Menedżera zasobów."
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
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Instalowanie i konfigurowanie MongoDB maszyny Linux platformy Azure
[MongoDB](http://www.mongodb.org) jest popularne Otwórz źródło, wysokiej wydajności NoSQL bazy danych. W tym artykule pokazano, jak zainstalować i skonfigurować MongoDB na maszyny Linux platformy Azure przy użyciu modelu wdrożenia Menedżera zasobów. Przykłady są wyświetlane tego szczegółów jak do:

- [Ręcznie zainstaluj i skonfiguruj podstawowe wystąpienie MongoDB](#manually-install-and-configure-mongodb-on-a-vm)
- [Tworzenie podstawowego wystąpienia MongoDB przy użyciu szablonu Menedżera zasobów](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Tworzenie złożonych MongoDB sharded klaster z replice zestawów przy użyciu szablonu Menedżera zasobów](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule należy spełnić następujące wymagania:

- Konto Azure ([Pobierz bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/)).
- [Polecenie Azure](../xplat-cli-install.md) podczas logowania`azure login`
- Polecenie Azure *musi być* przy użyciu trybu Menedżer zasobów Azure`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Ręcznie zainstaluj i skonfiguruj MongoDB na maszyny
MongoDB [dostarczanie instrukcji instalacji](https://docs.mongodb.com/manual/administration/install-on-linux/) distros Linux tym funkcję czerwony-CentOS, SUSE Ubuntu i Debian. Poniższy przykład tworzy `CoreOS` maszyn wirtualnych przy użyciu klucza SSH przechowywane w `.ssh/azure_id_rsa.pub`. Odbieranie monity o nazwę konta magazynu, nazwy DNS i poświadczenia administratora:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Zaloguj się do maszyn wirtualnych, przy użyciu publicznego adresu IP wyświetlane na końcu poprzedni krok tworzenia maszyn wirtualnych:

```bash
ssh ops@40.78.23.145
```

Aby dodać źródła instalacji MongoDB, Utwórz `yum` pliku repozytorium w następujący sposób:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Otwórz plik repo MongoDB do edycji. Dodaj następujące wiersze:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Instalowanie przy użyciu MongoDB `yum` w następujący sposób:

```bash
sudo yum install -y mongodb-org
```

Domyślnie SELinux są wymuszane na obrazach CentOS, które pozwala na uzyskiwanie dostępu do MongoDB. Zainstaluj narzędzia do zarządzania zasadami i skonfiguruj SELinux umożliwia MongoDB działania na jego domyślny port TCP 27017 w następujący sposób. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Uruchom usługę MongoDB w następujący sposób:

```bash
sudo service mongod start
```

Sprawdzanie instalacji MongoDB za pomocą nawiązywanie połączenia za pomocą lokalnej `mongo` klienta:

```bash
mongo
```

Teraz przetestuj wystąpienia MongoDB przez dodanie niektóre dane, a następnie wyszukiwania:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

W razie potrzeby należy skonfigurować MongoDB, aby była uruchamiana automatycznie podczas ponownego uruchomienia systemu:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Tworzenie podstawowego wystąpienia MongoDB na CentOS przy użyciu szablonu
Podstawowe wystąpienie MongoDB można tworzyć na jednym maszyn wirtualnych CentOS przy użyciu następującego szablonu Azure Szybki Start z Github. Aby dodać ten szablon używa rozszerzenia niestandardowego skryptu Linux `yum` repozytorium do nowo utworzonego CentOS maszyn wirtualnych, a następnie zainstaluj MongoDB.

- [Podstawowe MongoDB wystąpieniu na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `WestUS` region. Wprowadź własne wartości w następujący sposób:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] Polecenie Azure powrót do wiersza w ciągu kilku sekund tworzenia wdrożenia, ale instalacja i konfiguracja zajmuje kilka minut. Sprawdzanie stanu rozmieszczania z `azure group deployment show myResourceGroup`, w związku z tym wprowadzenie nazwy grupy zasobów. Poczekaj `ProvisioningState` przedstawia "Powiodło się" przed próbą SSH do maszyn wirtualnych.

Po zakończeniu rozmieszczania, SSH do maszyn wirtualnych. Uzyskać adres IP przy użyciu maszyn wirtualnych `azure vm show` polecenie tak jak w poniższym przykładzie:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Na końcu dane wyjściowe `Public IP address` jest wyświetlane. SSH do swojego maszyn wirtualnych o adresie IP usługi maszyn wirtualnych:

```bash
ssh ops@138.91.149.74
```

Sprawdzanie instalacji MongoDB za pomocą nawiązywanie połączenia za pomocą lokalnej `mongo` klienta w następujący sposób:

```bash
mongo
```

Teraz przetestuj wystąpienie, dodając niektóre dane i wyszukiwanie w następujący sposób:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Tworzenie złożonych klaster Sharded MongoDB na CentOS przy użyciu szablonu
Możesz utworzyć złożone MongoDB sharded klaster za pomocą następującego szablonu Azure Szybki Start z Github. Ten szablon wykonuje [MongoDB sharded klaster najważniejsze wskazówki](https://docs.mongodb.com/manual/core/sharded-cluster-components/) zapewnienie nadmiarowości i wysokiej dostępności. Szablon utworzy dwa odłamki z trzy węzły w każdym zestawie replice. Jednej konfiguracji serwera replice zestaw z trzy węzły również utworzeniu plus dwa `mongos` serwerów zapewniają zgodność aplikacji przez odłamki.

- [Klaster Sharding MongoDB na CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Wdrażanie złożonych klaster sharded MongoDB wymaga więcej niż 20 rdzenie, co jest typowe domyślny licznik core rozbiciu na regiony dla subskrypcji. Otwórz wezwanie Azure pomocy technicznej, aby zwiększyć liczba podstawowych.

Poniższy przykład tworzy grupę zasobów o nazwie `myResourceGroup` w `WestUS` region. Wprowadź własne wartości w następujący sposób:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] Polecenie Azure powrót do wiersza w ciągu kilku sekund tworzenia wdrożenia, ale instalacja i konfiguracja może zająć ponad godzinę do wykonania. Sprawdzanie stanu rozmieszczania z `azure group deployment show myResourceGroup`, dostosowując odpowiednio nazwę grupy zasobów. Poczekaj `ProvisioningState` przedstawia "Powiodło się" przed nawiązaniem połączenia maszyny wirtualne.


## <a name="next-steps"></a>Następne kroki
W tych przykładach możesz połączyć się z MongoDB wystąpieniem lokalnie z maszyn wirtualnych. Jeśli chcesz połączyć się z wystąpieniem MongoDB z innego maszyn wirtualnych lub sieci, upewnij się, odpowiedniej [grupy zabezpieczeń sieci reguły są tworzone](virtual-machines-linux-nsg-quickstart.md).

Aby uzyskać więcej informacji o tworzeniu korzystania z szablonów zobacz [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md).

Szablony Menedżera zasobów Azure należy użyć rozszerzenia skrypt niestandardowy Aby pobrać i uruchomić skrypty na pośrednictwem usługi SMS. Aby uzyskać więcej informacji zobacz [Korzystanie z rozszerzeniem skrypt niestandardowy Azure z maszyn wirtualnych Linux](virtual-machines-linux-extensions-customscript.md).