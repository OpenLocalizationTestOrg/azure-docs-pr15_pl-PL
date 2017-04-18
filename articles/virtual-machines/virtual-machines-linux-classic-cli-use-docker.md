<properties
    pageTitle="Przy użyciu rozszerzenia maszyn wirtualnych Docker Linux Azure"
    description="W tym artykule opisano Docker i rozszerzenia maszyn wirtualnych Azure i przedstawiono sposób programowy tworzenia maszyn wirtualnych Azure są hosts docker z poziomu wiersza polecenia przy użyciu interfejsu wiersza polecenia Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Za pomocą rozszerzenie maszyn wirtualnych Docker z wiersza polecenia Azure interfejsu (polecenie Azure)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



W tym temacie opisano sposób tworzenia maszyny z rozszerzeniem Docker maszyn wirtualnych w trybie zarządzania (asm) usługi w polecenie Azure na wszystkich platformach. [Docker](https://www.docker.com/) jest jedną z najpopularniejszych metod wirtualizacji używanych [kontenerów Linux](http://en.wikipedia.org/wiki/LXC) zamiast w środowisku maszyn wirtualnych systemu sposób izolowanie danych i korzystania z komputera do zasobów udostępnionych. Rozszerzenie Docker maszyn wirtualnych i [Agenta Linux Azure](virtual-machines-linux-agent-user-guide.md) umożliwia tworzenie maszyny Docker obsługującego dowolną liczbę kontenerów dla aplikacji Azure. Aby uzyskać omówienie wysokiego poziomu kontenerów i ich korzyści, zobacz [Docker wysoki poziom tablicy](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Jak korzystać z rozszerzeniem maszyn wirtualnych Docker z platformy Azure
Aby użyć rozszerzenia Docker maszyn wirtualnych z Azure, należy zainstalować wersję [Azure interfejsu wiersza polecenia](https://github.com/Azure/azure-sdk-tools-xplat) (polecenie Azure) wyższymi niż 0.8.6 (jak ten uchwyt bieżącej wersji jest 0.10.0). Polecenie Azure można zainstalować na komputerze Mac, Linux i systemu Windows.


Zakończ proces używania Docker na Azure jest proste:

+ Instalowanie na komputerze, z którego chcesz sterowania Azure polecenie Azure i jego zależności (w systemie Windows, będzie rozkładu Linux działającego jako maszyny wirtualnej)
+ Tworzenie hosta Docker maszyn wirtualnych w Azure przy użyciu poleceń Docker polecenie Azure
+ Lokalne polecenia Docker umożliwia zarządzanie kontenerów Docker w swojej maszyn wirtualnych Docker platformy Azure.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Instalowanie Azure interfejs wiersza polecenia (polecenie Azure)

Aby zainstalować i skonfigurować polecenie Azure, zobacz [jak zainstalować Azure wiersza polecenia](../xplat-cli-install.md). Aby sprawdzić, czy, wpisz `azure` w wierszu polecenia i po chwili krótki powinna być widoczna clipart Azure polecenie ASCII, które list podstawowe polecenia dostępne dla Ciebie. Jeśli instalacja nie działa poprawnie, powinno być możliwe wpisywanie `azure help vm` i sprawdź, czy jest jedno z wymienionych poleceń "docker".

> [AZURE.NOTE] Docker zawiera narzędzia dla systemu Windows, [Docker komputera](https://docs.docker.com/installation/windows/), które umożliwia także zautomatyzować tworzenie klienta docker, który służy do pracy z maszyny wirtualne Azure jako docker hosts.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>Azure polecenie, aby nawiązać połączenie konto Azure
Zanim będzie można używać polecenie Azure swoje poświadczenia konta Azure należy skojarzyć z polecenie Azure na platformie. Sekcja [jak połączyć do subskrypcji usługi Azure](../xplat-cli-connect.md) wyjaśniono, jak pobrać i zaimportować plik **.publishsettings** lub skojarzyć polecenie usługi Azure identyfikator organizacji.

> [AZURE.NOTE] Istnieją pewne różnice w zachowanie podczas korzystania z jedną lub inne metody uwierzytelniania, więc należy przeczytać powyżej, aby poznać inne funkcje dokumentu.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Instalowanie Docker i korzystanie z rozszerzeniem maszyn wirtualnych Docker Azure
Postępuj zgodnie z [instrukcjami instalacji Docker](https://docs.docker.com/installation/#installation) , aby zainstalować Docker lokalnie na komputerze.

Aby użyć Docker z maszyn wirtualnych Azure, obraz Linux na potrzeby maszyn wirtualnych musi być [Azure Linux maszyn wirtualnych agenta](virtual-machines-linux-agent-user-guide.md) zainstalowany. Obecnie są tylko dwa typy obrazów, które zapewniają to:

+ Obraz Ubuntu z galerii obrazów Azure lub

+ Linux obraz niestandardowy utworzony przy użyciu Azure Linux maszyn wirtualnych agenta zainstalowaniu i skonfigurowaniu. Aby uzyskać więcej informacji na temat sposobu tworzenia niestandardowych maszyn wirtualnych Linux z agentem maszyn wirtualnych Azure, zobacz [Agent maszyn wirtualnych Linux Azure](virtual-machines-linux-agent-user-guide.md) .

### <a name="using-the-azure-image-gallery"></a>Korzystanie z galerii obrazów Azure

Z imprezie lub sesja Użyj następującego polecenia polecenie Azure zlokalizować najnowszy obraz Ubuntu w galerii maszyn wirtualnych do użycia przez wpisanie tekstu

`azure vm image list | grep Ubuntu-14_04`

i wybierz jedną z nazw obrazów, takich jak `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`i użyj następującego polecenia do tworzenia nowych maszyn wirtualnych za pomocą tego obrazu.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

w przypadku gdy:

+ * &lt;nazwę maszyn wirtualnych cloudservice&gt; * to nazwa maszyn wirtualnych, który ma stać się kontener Docker hosta platformy Azure

+  * &lt;nazwa_użytkownika&gt; * to nazwa użytkownika domyślnego użytkownika głównego maszyn wirtualnych

+ * &lt;hasło&gt; * jest hasło konta *nazwy użytkownika* , która spełnia normy złożoności dla Azure

> [AZURE.NOTE] Obecnie hasło musi zawierać co najmniej 8 znaków, zawierają jedną małą literą i znak jeden wielkie litery, liczby i znaki specjalne, takie jak jedną z następujących znaków: `!@#$%^&+=`. Nie, okres na końcu zdania poprzedniego nie jest znak specjalny.

Jeśli polecenie zakończyło się pomyślnie, zobacz podobną do następujących opcji, w zależności od precyzyjnie argumenty i opcje, które zostało użyte:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] Tworzenie maszyny wirtualnej może potrwać kilka minut, ale po została przygotowana (wartość stanu jest `ReadyRole`) powoduje uruchomienie demona Docker (usługa Docker) i można połączyć się z hostem kontener Docker.

Aby przetestować maszyn wirtualnych Docker został utworzony w Azure, należy wpisać

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

miejsce, w którym * &lt;maszyn wirtualnych nazwa--użyta&gt; * jest nazwą maszyny wirtualnej, których użyto w połączenia do `azure vm docker create`. Powinien zostać wyświetlony podobnej do następujące dane, które oznacza, że maszyn wirtualnych usługi Docker hosta w górę i uruchomione w Azure i Oczekiwanie na polecenia. 

Teraz możesz nawiązać połączenie za pomocą klienta docker w celu uzyskania informacji (w niektórych ustawień klienta Docker, takich jak na komputerze Mac, może być konieczne używanie `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Po prostu, aby mieć pewność, że jest on wszystkie pracy, można sprawdzić maszyn wirtualnych dla rozszerzenia Docker:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker hosta maszyn wirtualnych uwierzytelniania

Oprócz tworzenia Głosowa Docker, `azure vm docker create` polecenie automatycznie tworzy wymagane certyfikaty umożliwia komputer kliencki Docker połączyć się z hostem Azure kontenera przy użyciu protokołu HTTPS, a certyfikaty są przechowywane na komputerach z klientem i hosta, zależnie od potrzeb. Na kolejne próby istniejących certyfikaty są ponownie wykorzystywać i udostępnione dla nowego hosta.

Domyślnie certyfikaty są umieszczane w `~/.docker`, i Docker zostanie skonfigurowany do uruchomienia na port **2376**. Jeśli chcesz użyć innego portu lub katalogu, a następnie możesz skorzystać z jednej z następujących `azure vm docker create` opcje wiersza polecenia, aby skonfigurować do kontenera Docker hosta maszyn wirtualnych, aby użyć innego portu lub różnych certyfikatów klientów nawiązujących połączenie:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Demona Docker na hoście jest skonfigurowany do wykrywać i uwierzytelniania połączeń klienta na określony port wykorzystaniem certyfikatów generowanych przez `azure vm docker create` polecenia. Komputer kliencki musi mieć te certyfikaty, aby uzyskać dostęp do hosta Docker.

> [AZURE.NOTE] Host podłączonym bez te certyfikaty będą występować do innych osób, które można połączyć się z komputerem. Przed zmodyfikowaniem konfiguracji domyślnej, upewnij się, rozumiesz kwestie zagrożeń do komputerów i aplikacji.

## <a name="next-steps"></a>Następne kroki

* Możesz przystąpić do Przechodzenie do [Docker User Guide] i używanie maszyn wirtualnych usługi Docker. Aby utworzyć maszyn wirtualnych z obsługą Docker w portalu nowy, zobacz [jak korzystać z rozszerzeniem maszyn wirtualnych Docker Portal].

* Rozszerzenie maszyn wirtualnych Docker Azure obsługuje także zredagować Docker, którego korzysta deklaracyjnych pliku YAML, aby pobrać aplikację modelowania Deweloper w każdym środowisku i spójne wdrażanie wygenerowania. Zobacz [Rozpoczynanie pracy z Docker i redagowanie zdefiniować i uruchom aplikację kontenera wielu elementów na Azure maszyn wirtualnych].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[Jak korzystać z rozszerzeniem maszyn wirtualnych Docker z portalu]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Przewodnik użytkownika docker]: https://docs.docker.com/userguide/
 
[Rozpoczynanie pracy z Docker i redagowanie, aby zdefiniować i uruchom aplikację kontenera wielu elementów na Azure maszyn wirtualnych]:virtual-machines-linux-docker-compose-quickstart.md