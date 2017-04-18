<properties
   pageTitle="Docker i redagowanie na komputerze wirtualnych | Microsoft Azure"
   description="Krótkie wprowadzenie do pracy z Redagowanie i Docker w przypadku maszyn wirtualnych Linux platformy Azure"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Rozpoczynanie pracy z Docker i redagowanie, aby zdefiniować i uruchom aplikację kontenera wielu elementów na Azure maszyn wirtualnych

Rozpoczynanie pracy przy użyciu funkcji Docker i [Redagowanie](http://github.com/docker/compose) definiować i wykonywać złożonej aplikacji na komputerze wirtualnych Linux platformy Azure. Z redagowanie plik tekstowy jest służy do definiowania aplikacja składająca się z wielu kontenerów Docker. Następnie pokrętła się na aplikację pojedynczego polecenia, które jest wdrożenia zdefiniowane środowiska. 

Na przykład w tym artykule pokazano, jak szybko skonfigurować blogu WordPress z bazą danych MariaDB SQL na maszyn wirtualnych Ubuntu wewnętrznej bazie danych. Redagowanie umożliwia również skonfigurować bardziej złożonych aplikacji.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Krok 1: Konfigurowanie maszyny Linux jako Docker host

Umożliwia różnych procedur Azure i dostępne obrazy lub szablony Menedżera zasobów w Azure Marketplace tworzenie maszyny Linux i ustawić ją jako Docker host. Na przykład zobacz [przy użyciu rozszerzenia maszyn wirtualnych Docker wdrożenia środowiska](virtual-machines-linux-dockerextension.md) szybkie tworzenie maszyn wirtualnych Ubuntu z rozszerzeniem maszyn wirtualnych Docker Azure za pomocą [szablonu Szybki Start](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Korzystając z rozszerzeniem Docker maszyn wirtualnych usługi maszyn wirtualnych automatycznie jest skonfigurowana jako Docker host i redagowanie jest już zainstalowany. Przykład w tym artykule pokazano, jak używać [Azure interfejs wiersza polecenia dla komputerów Mac, Linux i systemu Windows](../xplat-cli-install.md) (polecenie Azure) w trybie Menedżera zasobów, aby utworzyć maszyn wirtualnych.

Podstawowe polecenia z poprzedniego dokumentu tworzy grupę zasobów o nazwie `myResourceGroup` w `West US` lokalizacji i wdrożeniu jej maszyny z rozszerzeniem maszyn wirtualnych Docker Azure zainstalowany:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Krok 2: Sprawdź, czy zainstalowano redagowanie

Po ukończeniu wdrażanie SSH do hosta Docker za pomocą DNS nazwy zostanie podany podczas wdrażania. Możesz użyć `azure vm show -g myDockerResourceGroup -n myDockerVM` Aby wyświetlić szczegóły swojego Głosowa, łącznie z nazwą DNS.

Aby sprawdzić, czy na maszyn wirtualnych zainstalowano redagowanie, uruchom następujące polecenie:

```bash
docker-compose --version
```

Zobacz dane wyjściowe podobne do `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Jeśli używasz innej metody tworzenia hosta Docker i musisz zainstalować redagowanie samodzielnie można znaleźć w [dokumentacji redagowanie](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Krok 3: Tworzenie pliku konfiguracji docker compose.yml

Następnie należy utworzyć `docker-compose.yml` pliku, który jest po prostu konfiguracji pliku tekstowego, aby zdefiniować kontenerów Docker do uruchomienia na maszyn wirtualnych. Plik Określa obraz, aby uruchomić na każdym kontenerze (lub może być kompilacji z Dockerfile), zmienne potrzeby środowiska i współzależności, porty i łącza między kontenerów. Aby uzyskać szczegółowe informacje o składni pliku yml zobacz [informacje dotyczące pliku redagowanie](http://docs.docker.com/compose/yml/).

Tworzenie `docker-compose.yml` pliku w następujący sposób:

```bash
touch docker-compose.yml
```

Dodawanie danych do pliku za pomocą edytora tekstu Ulubione. W poniższym przykładzie użyto `vi` edytora:

```bash
vi docker-compose.yml
```

Poniższy przykład wkleić do pliku tekstowego. Ta konfiguracja używa obrazów z [Rejestru DockerHub](https://registry.hub.docker.com/_/wordpress/) do zainstalowania WordPress (Otwórz źródło blogi i zawartość system zarządzania) i połączonej bazy danych MariaDB SQL wewnętrznej bazie danych. Wprowadzanie własnych `MYSQL_ROOT_PASSWORD` w następujący sposób:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Krok 4: Początkowych redagowanie kontenerów

W tym samym katalogu co do `docker-compose.yml` pliku, uruchom następujące polecenie (w zależności od środowiska, może być konieczne uruchamianie `docker-compose` za pomocą `sudo`.):

```bash
docker-compose up -d

```

To polecenie uruchamia kontenerów Docker określonego w `docker-compose.yml`. Wystarczy minutę lub dwie dla tej czynności do wykonania. Zostanie wyświetlone dane wyjściowe podobne do następującego przykładu:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Należy użyć opcji **-d** przy uruchamianiu tak, aby uruchomić w tle stale kontenerów.

Aby sprawdzić, czy kontenerów w górę, wpisz `docker-compose ps`. Powinien zostać wyświetlony mniej więcej tak:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Teraz można nawiązać WordPress bezpośrednio na maszyn wirtualnych na porcie 80. Otwórz przeglądarkę sieci web i wpisz nazwę serwera DNS usługi maszyn wirtualnych (takie jak `http://myresourcegroup.westus.cloudapp.azure.com`). WordPress powinien zostać wyświetlony ekran startowy, gdzie można ukończyć instalację i rozpocząć pracę z aplikacją.

![Ekran startowy WordPress][wordpress_start]


## <a name="next-steps"></a>Następne kroki

* Przejdź do [Podręcznik użytkownika rozszerzenia maszyn wirtualnych Docker](https://github.com/Azure/azure-docker-extension/blob/master/README.md) Aby uzyskać więcej opcji skonfigurować Docker i redagowanie w maszyn wirtualnych usługi Docker. Na przykład jedna opcja jest umieszczony plik yml redagowanie (przekonwertowana na JSON) bezpośrednio w konfiguracji z rozszerzeniem Docker maszyn wirtualnych.
* Zapoznaj się z [wiersza polecenia redagowanie](http://docs.docker.com/compose/reference/) i [Podręcznik użytkownika](http://docs.docker.com/compose/) , aby uzyskać więcej przykładów tworzenie i wdrażanie aplikacji kontenera wielu.
* Za pomocą szablonu Menedżera zasobów Azure albo usługi samodzielnie lub jeden przyczynić się ze [społeczności](https://azure.microsoft.com/documentation/templates/)do wdrażania maszyn wirtualnych Azure z Docker i skonfigurować, używając redagowanie aplikacji. Na przykład szablon [Rozmieszczanie blogu WordPress z Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) używa Docker i redagowanie Aby szybko wdrożyć WordPress wewnętrznej bazy danych MySQL na Ubuntu maszyn wirtualnych.
* Spróbuj integracji Zredaguj Docker z klastrem [Docker Swarm](virtual-machines-linux-docker-swarm.md) . Zobacz [Przy użyciu Zredaguj z rój](https://docs.docker.com/compose/swarm/) scenariuszach.

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png
