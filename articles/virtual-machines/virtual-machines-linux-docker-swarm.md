<properties
   pageTitle="Wprowadzenie do docker za pomocą rój Azure"
   description="W tym artykule opisano, jak utworzyć grupę maszyny wirtualne z rozszerzeniem maszyn wirtualnych Docker i umożliwia tworzenie klastrze Docker rój."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Jak używać docker z rój

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


W tym temacie przedstawiono bardzo prosty sposób za pomocą [docker](https://www.docker.com/) [swarm](https://github.com/docker/swarm) można utworzyć klaster zarządzane rój Azure. Tworzy cztery maszyn wirtualnych w Azure, jedną ma pełnić rolę menedżera rój i trzy jako część klaster hostów docker. Po zakończeniu, umożliwia rój Zobacz klaster i zacznij używać docker nad nim. Ponadto połączenia polecenie Azure w tym temacie tryb usługi zarządzania (asm). 

> [AZURE.NOTE] W tym temacie używa docker z rój i polecenie Azure *bez* przy użyciu **docker komputera** , aby można było wyświetlić sposoby współdziałania różnych narzędzi, ale pozostają niezależne. **komputer docker** ma **— swarm** przełączniki umożliwiające bezpośrednio dodać węzły do rój za pomocą **komputera docker** . Na przykład zapoznaj się z dokumentacją [docker komputera](https://github.com/docker/machine) . Jeśli nie odebrano **komputera docker** uruchomionym w celu maszyny wirtualne Azure, zobacz [jak za pomocą docker maszyny liczącej z Azure](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Tworzenie docker hosts z maszyn wirtualnych Azure

W tym temacie tworzy cztery maszyny wirtualne, ale można użyć dowolnego numeru, który ma. Nawiązywanie połączenia następujących * &lt;hasło&gt; * zastępuje hasła wybrano.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Po zakończeniu należy wyświetlić maszyny wirtualne usługi Azure za pomocą **listy azure maszyn wirtualnych** :

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Instalowanie rój we wzorcu rój maszyn wirtualnych

W tym temacie używa [modelu kontenera instalacji z docker swarm dokumentacji](https://github.com/docker/swarm#1---docker-image) —, ale możesz także SSH **rój wzorca**. W tym modelu **swarm** są pobierane jako kontenera docker uruchomiony rój. Poniżej możemy wykonać ten krok *zdalnie z naszych przenośnego przy użyciu docker* nawiązywanie **wzorzec rój** maszyn wirtualnych i określić go za pomocą polecenia tworzenia identyfikator klaster, **Tworzenie rój**. Identyfikator klaster jest, jak **swarm** wykryje członków grupy rój. (Można także klonowanie repozytorium i tworzą go samodzielnie, które zapewniają pełną kontrolę nad oraz włączyć debugowanie.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Aby ostatni wiersz jest identyfikator klaster; Skopiuj go innym ponieważ użyjesz go ponownie podczas dołączania do węzła maszyny wirtualne do wzorca rój w celu utworzenia "rój". W tym przykładzie identyfikator klaster jest **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Tak, aby mieć Wyczyść, użyto naszych instalacji lokalnych docker nawiązać **wzorzec rój** maszyn wirtualnych Azure i instrukcje **wzorzec rój** pobieranie, instalowanie i uruchom polecenie **Utwórz** , która zwraca identyfikator naszych klaster, używany na potrzeby odnajdowania później.
<!-- -->
> Aby to sprawdzić, uruchom `docker -H tcp://` * &lt;hostname&gt; * ` images` do wyświetlania listy procesów kontener na komputerze **rój wzorca** i w innym węźle porównania (ponieważ zostało poprzedniego polecenia rój z przełącznikiem **— Menedżera zasobów** , kontener został usunięty po jego zakończeniu, za pomocą **docker ps —** nie zwrócą żadnych).:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Jeśli znasz **docker**wiadomo, czy inne węzły mają żadnych wpisów, ponieważ pobraniu i uruchomić jeszcze nie obrazów.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Dołączanie do węzła maszyny wirtualne do klastrów naszych docker

Dla każdego węzła listy informacji punktu końcowego przy użyciu interfejsu wiersza polecenia Azure. Poniżej czynności dla hosta docker **rój węzeł-1** w celu uzyskania portu docker węzła.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Za pomocą **docker** oraz `-H` opcję, aby wskazywały klienta docker węzeł Głosowa, aby rój tworzysz przekazując identyfikator klaster oraz portu docker węzła sprzężeń węzła (jego przy użyciu **— adres**):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Który odpowiada Twoim potrzebom. Aby potwierdzić, że **swarm** jest uruchomiony na **rój węzeł-1** , które możemy wpisz:

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Powtórz dla wszystkich innych węzłach w klastrze. W naszym przypadku możemy zrobić **rój węzeł 2** i **rój węzeł-3**.

## <a name="begin-managing-the-swarm-cluster"></a>Rozpocznij zarządzanie klastrem rój

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

a następnie można wyświetlić listę się węzły w klastrze:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Następne kroki

Uruchamianie poleceń na swojej rój podróży. Aby wyszukać pomysłów, zobacz [https://github.com/docker/swarm/](https://github.com/docker/swarm/)lub prawdopodobnie [wideo](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 
