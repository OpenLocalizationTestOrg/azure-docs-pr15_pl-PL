<properties
   pageTitle="Wdrażanie węźle 3 Deis klaster | Microsoft Azure"
   description="Ten artykuł zawiera opis sposobu tworzenia węźle 3 Deis klaster Azure za pomocą szablonu Azure Menedżera zasobów"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="HaishiBai"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# <a name="deploy-a-3-node-deis-cluster"></a>Wdrażanie węźle 3 Deis klaster

W tym artykule opisano za pośrednictwem inicjowania obsługi administracyjnej [Deis](http://deis.io/) klaster Azure. Obejmuje wszystkie kroki z Tworzenie certyfikatów niezbędne do wdrażania i skalowanie przykładowej aplikacji **Przejdź** na nowo ustanawianie klaster.

Na poniższym diagramie przedstawiono architektury wdrożonym systemu. Administrator systemu zarządza klaster, za pomocą narzędzi, takich jak **deis** i **deisctl**Deis. Połączenia zostały ustanowione za pośrednictwem usługi równoważenia obciążenia Azure, który przekazuje połączenia do jednego członka węzły w klastrze. Klientom dostęp wdrożyć aplikacje za pomocą usługę równoważenia obciążenia. W tym przypadku równoważenia obciążenia przekazuje ruch do Deis siatki router, które dodatkowo routs ruch do odpowiednich kontenerów Docker uruchomiona w klastrze.

  ![Diagram architektury wdrożonym klastrze Desis](media/virtual-machines-linux-deis-cluster/architecture-overview.png)

Aby uruchomić przez następujące etapy, musisz:

 * Aktywną subskrypcję Azure. Jeśli nie masz, możesz uzyskać bezpłatną dziennik na [azure.com](https://azure.microsoft.com/).
 * Identyfikator służbowego do używania grup zasobów Azure. Jeśli masz konto osobiste i zaloguj się przy użyciu identyfikatora Microsoft, należy [utworzyć identyfikator pracy niż osobistych](virtual-machines-windows-create-aad-work-id.md).
 * Albo — w zależności od systemu operacyjnego klienta — [Azure programu PowerShell](../powershell-install-configure.md) lub [Interfejsu wiersza polecenia Azure dla komputerów Mac, Linux i systemu Windows](../xplat-cli-install.md).
 * [OpenSSL](https://www.openssl.org/). OpenSSL służy do generowania wymagane certyfikaty.
 * Klient cyfra, takich jak [Imprezie cyfra](https://git-scm.com/).
 * Aby przetestować przykładowej aplikacji, również musisz serwera DNS. Za pomocą serwerów DNS ani usług, które obsługują rekordy A symboli wieloznacznych.
 * Komputer, aby uruchomić Deis narzędziami klienta. Za pomocą komputera lokalnego lub maszyny wirtualnej. Te narzędzia można uruchomić na dowolnym rozmieszczenia Linux, ale poniższe instrukcje użyć Ubuntu.

## <a name="provision-the-cluster"></a>Obsługa administracyjna klaster

W tej sekcji za pomocą szablonu [Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md) z repozytorium Otwórz źródło [azure — Szybki Start szablony](https://github.com/Azure/azure-quickstart-templates). Najpierw zostanie skopiuj szablon w dół. Następnie utworzysz nowy SSH parę kluczy uwierzytelniania. A następnie, będzie skonfiguruj nowy identyfikator dla Ciebie klaster. A na koniec użyjesz skrypt powłoki lub skrypt programu PowerShell do zapewniania obsługi klaster.

1. Klonowanie repozytorium: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).

        git clone https://github.com/Azure/azure-quickstart-templates

2. Przejdź do folderu szablonu:

        cd azure-quickstart-templates\deis-cluster-coreos

3. Utwórz nową parę kluczy SSH przy użyciu ssh-keygen:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. Generowanie certyfikatu przy użyciu podanych klucz prywatny:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. Przejdź do [https://discovery.etcd.io/new](https://discovery.etcd.io/new) do wygenerowania nowego tokenu klaster, który wygląda na przykład:

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
Każdy klaster CoreOS musi mieć unikatowy token z tej bezpłatnej usługi. Aby uzyskać więcej informacji, zobacz [dokumentację CoreOS](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) .

6. Zmodyfikuj **chmury config.yaml** Aby zamienić istniejące token **odnajdowania** nowy token:

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. Modyfikowanie pliku **azuredeploy parameters.json** : Otwórz certyfikat utworzony w kroku 4 w edytorze tekstów. Kopiowanie całego tekstu między `----BEGIN CERTIFICATE-----` i `-----END CERTIFICATE-----` w parametrze **sshKeyData** (musisz usunąć wszystkie znaki nowego wiersza).

8. Modyfikowanie parametru **newStorageAccountName** . To jest konto miejsca do magazynowania dla maszyn wirtualnych systemu operacyjnego dysków. Nazwa tego konta musi być globalnie unikatowe.

9. Modyfikowanie parametru **publicDomainName** . Będzie część nazwy DNS skojarzone z IP publicznej usługi równoważenia obciążenia. Ostateczne FQDN będą mieć format _[wartość ten parametr]_. _[region]_. cloudapp.azure.com. Na przykład jeśli Określ nazwę jako deishbai32 i grupa zasobów zostanie wdrożony w regionie Zachód USA, ostateczne FQDN do usługi równoważenia obciążenia będzie deishbai32.westus.cloudapp.azure.com.

10. Zapisz plik parametru. A następnie umożliwia obsługę klaster przy użyciu programu PowerShell Azure:

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  lub polecenie Azure:

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. Po ponieważ jest ono inicjowane grupa zasobów, można wyświetlić wszystkie zasoby w grupie na portal Azure klasyczny. Jak pokazano na poniższej zrzut ekranu, grupa zasobów zawiera wirtualnej sieci z trzech maszyny wirtualne, połączonych do tego samego zestawu dostępności. Grupa zawiera również równoważenia obciążenia, który ma skojarzony publiczny adres IP.

  ![Grupa ustanawianie zasobów portalu klasyczny Azure](media/virtual-machines-linux-deis-cluster/resource-group.png)

## <a name="install-the-client"></a>Instalowanie klienta pakietu

Potrzebujesz **deisctl** do kontrolki programu Deis klaster. Chociaż deisctl jest instalowane automatycznie w węzłach klaster, jest zaleca się za pomocą deisctl na osobnym komputerze administracyjne. Ponadto ponieważ wszystkie węzły skonfigurowano tylko prywatnych adresów IP, musisz za pomocą SSH tunneling za pośrednictwem usługi równoważenia obciążenia, która ma publiczny adres IP, aby połączyć się z komputerami węzeł. Poniżej przedstawiono kroki konfigurowania deisctl na osobne Ubuntu fizycznej lub maszyn wirtualnych.

1. Zainstaluj deisctl:mkdir deis

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. Dodaj klucz prywatny, aby ssh agenta:

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. Konfigurowanie deisctl:

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

Szablon określa reguły przychodzące translatora adresów Sieciowych, które mapowanie 2223 do wystąpienia 1, 2224 wystąpieniu 2 i 2225 wystąpieniu 3. To nadmiarowości dotyczące korzystania z narzędzia deisctl. Można sprawdzić te reguły portalu klasyczny Azure:

![Zasady translatora adresów Sieciowych usługi równoważenia obciążenia](media/virtual-machines-linux-deis-cluster/nat-rules.png)

> [AZURE.NOTE] Obecnie szablon obsługuje tylko klastrów 3. Jest to ze względu na to ograniczenie w szablonie Menedżera zasobów Azure Definicja reguły translatora adresów Sieciowych, który nie obsługuje składni pętli.

## <a name="install-and-start-the-deis-platform"></a>Zainstaluj i uruchom Deis platformy

Teraz możesz wykorzystać deisctl, aby zainstalować i uruchomić Deis platformy:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] Uruchamianie platformie zajmie trochę czasu (jak 10 minut). Szczególnie uruchamianie usługi Konstruktor może zająć dużo czasu. I czasami wystarczy kilku prób powiodła się: jeśli operację wydaje się zawieszać, spróbuj wpisać `ctrl+c` aby przerwać wykonanie polecenia i spróbuj ponownie.

Możesz użyć `deisctl list` Aby sprawdzić, czy wszystkie usługi są uruchomione:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Gratulacje! Teraz masz bieżącą Deis clsuter Azure! Następnie Załóżmy wdrażanie aplikacji Przejdź, aby wyświetlić klaster w działaniu próbki.

## <a name="deploy-and-scale-a-hello-world-application"></a>Wdrażanie i skalowanie aplikacji Witaj świecie

Poniższe kroki pokazują jak wdrożyć "Witaj świecie" Przejdź aplikacji z klastrem. Kroki są oparte na [Deis dokumentacji](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. Kierowanie siatki, aby działał poprawnie musisz ma rekordu symbol wieloznaczny, A dla swojej domeny, wskazująca publiczny adres IP modułu równoważenia obciążenia. Następujące zrzut ekranu przedstawia rekord rejestracji domeny próbki w witrynie GoDaddy:

    ![GoDaddy rekord](media/virtual-machines-linux-deis-cluster/go-daddy.png)
<p />
2. Zainstaluj deis:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. Utwórz nowy klucz SSH, a następnie dodaj klucz publiczny do GitHub (oczywiście można też użyć ponownie kluczy istniejących). Aby utworzyć nową parę kluczy SSH, należy użyć:

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. Dodawanie do GitHub id_rsa.pub lub klucz publiczny dokonanego wyboru. Możesz to zrobić przy użyciu Dodawanie SSH klucza przycisku na ekranie konfiguracji SSH kluczy:

  ![Klucz Github](media/virtual-machines-linux-deis-cluster/github-key.png)
<p />
5. Rejestrowanie nowego użytkownika:

        deis register http://deis.[your domain]
<p />
6. Dodawanie klucza SSH:

        deis keys:add [path to your SSH public key]
  <p />      
7. Tworzenie aplikacji.

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8.Naciśnięcie cyfra spowoduje obrazów Docker wbudowane i wdrażania, które może potrwać kilka minut. Moje możliwości czasami krok 10 (Pushing obraz do repozytorium prywatnych) może się zawieszać. W takim przypadku możesz zatrzymać proces, usuwanie, przy użyciu aplikacji "deis aplikacji: destroy — <application name> ` to remove the application and try again. You can use `deis apps:list" Aby dowiedzieć się, nazwę aplikacji. Jeśli wszystko działa powinien zostać wyświetlony podobną do następujących na końcu wyjściowe polecenia:

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. Sprawdź, czy aplikacja działa:

        curl -S http://[your application name].[your domain]
  Powinien zostać wyświetlony:

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. Skala 3 wystąpień aplikacji:

        deis scale cmd=3
<p />
11. Opcjonalnie można użyć deis informacje, aby sprawdzić szczegóły aplikacji. Następujące wyjściowe pochodzą z moim wdrażanie aplikacji:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Następne kroki

W tym artykule udał wszystkie kroki do zapewniania obsługi nowego Deis klaster Azure za pomocą szablonu Azure Menedżera zasobów. Szablon obsługuje nadmiarowości w narzędzie połączeń, a także równoważenia obciążenia dla wdrożonych aplikacji. Szablon pozwala również uniknąć za pomocą publicznej adresy IP w węzłach członka, które umożliwia zapisanie cenne zasoby IP publicznej i oferuje bardziej zabezpieczone środowisko do obsługi aplikacji. Aby uzyskać więcej informacji, zobacz następujące artykuły:

[Omówienie Menedżera zasobów Azure] [resource-group-overview]  
[Jak używać polecenie Azure] [azure-command-line-tools]  
[Przy użyciu programu PowerShell Azure przy użyciu Menedżera zasobów Azure] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md
