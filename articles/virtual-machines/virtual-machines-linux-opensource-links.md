<properties
    pageTitle="Linux i obliczanie Azure Open Source | Microsoft Azure"
    description="Lista Linux i Otwórz źródło obliczania artykuły dotyczące Azure, w tym podstawowe użycie Linux i niektóre podstawowe pojęcia związane z uruchamianiem lub przekazywania obrazów Linux na Azure i inne informacje dotyczące określonych technologii i optymalizacje."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux i open source obliczeniowych Azure

Znajdowanie dokumentacji, potrzebne do tworzenia i zarządzania systemem Linux maszyn wirtualnych w modelu Klasyczny wdrożenia.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Rozpoczynanie pracy
- [Wprowadzenie Linux Azure](virtual-machines-linux-intro-on-azure.md)
- [Często zadawane pytania dotyczące Azure maszyn wirtualnych utworzone za pomocą modelu Klasyczny wdrażania](virtual-machines-linux-classic-faq.md)
- [Informacje o obrazach dla maszyn wirtualnych](virtual-machines-linux-classic-about-images.md)
- [Przekazywanie obrazu Distro](virtual-machines-linux-classic-create-upload-vhd.md) (i również instrukcje przy użyciu [Rozkładu Azure-Endorsed](virtual-machines-linux-endorsed-distros.md))
- [Zaloguj się do maszyn wirtualnych Linux za pomocą portalu klasyczny Azure](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Konfigurowanie

- [Instalowanie Azure interfejs wiersza polecenia (polecenie Azure)](../xplat-cli-install.md)


## <a name="tutorials"></a>Samouczki

- [Stos światło zostaną zainstalowane na komputerze wirtualnych Linux platformy Azure](virtual-machines-linux-create-lamp-stack.md)
- [Dopiskiem w aplikacji sieci Web szyn na maszyn wirtualnych Azure](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Jak: Instalowanie Apache Qpid protonów C AMQP i Bus usługi](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Bazy danych
- [Optymalizowanie MySQL Azure](virtual-machines-linux-classic-optimize-mysql.md)
- [Klastrów MySQL](virtual-machines-linux-classic-mysql-cluster.md)
- [Z Cassandra z systemem Linux Azure i uzyskiwanie dostępu do z Node.js](virtual-machines-linux-classic-cassandra-nodejs.md)
- [Tworzenie wielu wzorców klastrze MariaDbs](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Rozpoczynanie pracy z Linux węzły do uruchamiania w klastrze HPC Pack platformy Azure](virtual-machines-linux-classic-hpcpack-cluster.md)
- [Uruchamianie NAMD z Microsoft HPC Pack w węzłach obliczeń Linux platformy Azure](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Konfigurowanie klastrze Linux RDMA do uruchamiania aplikacji MPI](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Docker
- [Za pomocą rozszerzenie maszyn wirtualnych Docker z wiersza polecenia Azure interfejsu (polecenie Azure)](virtual-machines-linux-classic-cli-use-docker.md)
- [Przy użyciu rozszerzenia Docker maszyn wirtualnych z portalu Azure](virtual-machines-linux-classic-portal-use-docker.md)
- [Jak za pomocą komputera docker Azure](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [Jak: klastrów MySQL](virtual-machines-linux-classic-mysql-cluster.md)
- [Jak: Node.js i Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Jak: Instalowanie i uruchamianie MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [Jak: używanie CoreOS Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Planowanie
- [Zasady implementacji usługi Azure infrastruktury](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Wybieranie użytkowników Linux](virtual-machines-linux-usernames.md)
- [Jak skonfigurować dostępność ustawieniem maszyn wirtualnych w modelu Klasyczny wdrażania](virtual-machines-linux-classic-configure-availability.md)
- [Jak planować planowanej konserwacji na Azure maszyny wirtualne](virtual-machines-linux-planned-maintenance-schedule.md)
- [Zarządzanie dostępność maszyn wirtualnych](virtual-machines-linux-manage-availability.md)
- [Planowanej konserwacji dla maszyn wirtualnych Linux platformy Azure](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Wdrożenie
- [Tworzenie niestandardowego maszyny wirtualnej systemem Linux](virtual-machines-linux-classic-createportal.md)
- [Podstawy: Przechwytywanie maszyny Linux w celu utworzenia szablonu](virtual-machines-linux-classic-capture-image.md)
- [Informacje dotyczące innych niż zatwierdzone dystrybucji](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Zarządzanie

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Jak zresetować hasło lub właściwości SSH Linux](virtual-machines-linux-classic-reset-access.md)
- [Za pomocą katalogu głównego](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Zasoby Azure

- [Azure agenta Linux](virtual-machines-linux-agent-user-guide.md)
- [Rozszerzenia Azure maszyn wirtualnych i funkcje](virtual-machines-windows-extensions-features.md)
- [Wstrzykiwania dane niestandardowej do maszyn wirtualnych do użytku z chmury inicjowania](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Miejsca do magazynowania

- [Dołączanie dysku danych do maszyny Linux](virtual-machines-linux-classic-attach-disk.md)
- [Odłączanie dysku danych z maszyny Linux](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Sieci
- [Jak skonfigurować punkty końcowe w klasycznym maszyny wirtualnej platformy Azure](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Rozwiązywanie problemów
- [Rozwiązywanie problemów z połączenia Secure Shell (SSH) z systemem Linux maszyny wirtualnej Azure](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Rozwiązywanie problemów klasyczny wdrożenia do tworzenia nowych maszyny wirtualnej Linux platformy Azure](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Rozwiązywanie problemów klasyczny wdrażania z ponownie lub zmienianie rozmiaru istniejące maszyny wirtualnej Linux platformy Azure](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Odwołania

- [Azure poleceń interfejsu wiersza polecenia w trybie Zarządzanie usługą Azure (asm)](../virtual-machines-command-line-tools.md)
- [Zarządzanie usługą Azure interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Ogólne łącza
Poniższe łącza będą blogów firmy Microsoft i stron w witrynie Technet i zewnętrznych witryn zamiast dokumentacji Azure.com co powyżej. Jak zarówno Azure i świecie komputerowym Otwórz źródło szybko celów, jest prawie pewne, że poniższe łącza są poza daty, *Mimo* fakt, że są czynności najlepsze ciągłe Dodawanie nowszego tematy i usuwanie z nich nieaktualne. Jeśli firma Microsoft zostały odebrane jedną, sprawdź Pozwól nam znasz w komentarzach lub Prześlij żądanie pobieraj do naszych [GitHub repo](https://github.com/Azure/azure-content/).

- [Uruchomionych Linux przy użyciu kontenerów Docker 5 programu ASP.NET](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Jak wdrożyć obraz maszyn wirtualnych CentOS z OpenLogic](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [Infrastruktura aktualizacji SUSE](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SUSE Linux Enterprise Server dla biblioteki urządzenia chmury SAP](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Tworzenie wysokiej dostępności Linux Azure w krokach 12](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Automatyzowanie inicjowania obsługi administracyjnej Linux Azure polecenie Azure, node.js, jhawk](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [GlusterFS Azure](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [Uruchamianie FreeBSD platformy Azure](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Łatwe wdrażanie FreeBSD](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Wdrażanie niestandardowych obrazu FreeBSD](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky AV serwera plików Linux](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [8 NoSql Otwórz źródło baz danych platformy Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Doświadczeń z CouchDb Azure](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [Usługi CouchDB jako z-z node.js, CORS i Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Redis w systemie Windows w usłudze Azure Redis pamięci podręcznej](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [Informowanie o dostawca stanu sesji ASP.NET dla wersji Preview Redis](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Blog: RavenHQ teraz dostępne w Azure Marketplace](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Duży danych
- [Instalowanie Hadoop na maszyny wirtualne Azure Linux](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Usługa Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relacyjne bazy danych
- [Architektura dostępność wysoki MySQL platformy Microsoft Azure](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Instalowanie Postgres z corosync, pg_bouncer za pomocą ILB](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux wysokiej wydajności (HPC) komputerowe

- [Szablonu Szybki Start: aż klastrze SLURM](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (i [Ogłoszenie w blogu](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [Szablon Szybki Start: tworzenie klastrze HPC węzłów obliczeń Linux](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, zarządzanie i optymalizacja

Jako świata devops zarządzanie i optymalizację jest bardzo rozszerzania i bardzo szybko zmienić, należy rozważyć wymienione poniżej punktu początkowego.

- [Klip wideo: Azure maszyn wirtualnych: za pomocą Kuchmistrz, Puppet i Docker zarządzania Linux maszyny wirtualne](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Pełne informacje automatycznego wdrażania klaster Kubernetes z CoreOS i splot](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Wizualizator Kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Jenkins podrzędna wtyczkę w przypadku platformy Azure](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub repo: Jenkins magazynowania wtyczkę w przypadku platformy Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Innej firmy: Wtyczkę w przypadku Azure podrzędna Hudson](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Trzecią: Hudson miejsca do magazynowania wtyczkę w przypadku platformy Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Klip wideo: Co to jest Kuchmistrz i jak to działa?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Klip wideo: Jak automatyzacji Azure za pomocą maszyny wirtualne Linux](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Blog: Jak wykonać DSC programu Powershell Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Docker klienta DSC](https://github.com/anweiss/DockerClientDSC)

- [Dodatek pakujący dla Azure](https://github.com/msopentech/packer-azure)
