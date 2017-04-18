<properties 
   pageTitle="Niepowodzenie wstecz maszyn wirtualnych VMware i serwerów fizycznych do lokalnej witryny | Microsoft Azure"
   description="Informacje na temat błędu powrót do lokalnej witryny po przełączeniu maszyny wirtualne VMware i serwerów fizycznych Azure." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required" 
   ms.date="08/22/2016"
   ms.author="ruturajd"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Niepowodzenie wstecz maszyn wirtualnych VMware i serwerów fizycznych do lokalnej witryny

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-failback-azure-to-vmware.md)
- [Portal Azure klasyczny](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure portalu klasyczny (starsza wersja)](site-recovery-failback-azure-to-vmware-classic-legacy.md)



W tym artykule opisano, jak niepowodzenie ponownie Azure maszyn wirtualnych z platformy Azure do lokalnej witryny. Postępuj zgodnie z instrukcjami w tym artykule, gdy użytkownik chce powrócić maszyn wirtualnych VMware lub serwerów fizycznych systemu Windows i Linux oraz po ich zostały nie powiodło się nad ze źródeł lokalnych witryny Azure za pomocą tego [samouczka](site-recovery-vmware-to-azure-classic.md).



## <a name="overview"></a>Omówienie

Ten diagram przedstawia architektura awarii w tym scenariuszu.

Jeśli serwer proces lokalnego i używasz ExpressRoute za pomocą tej architektury.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Jeśli serwer proces Azure i korzystasz z sieci VPN lub połączenie ExpressRoute za pomocą tej architektury.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Aby wyświetlić pełną listę porty i protokoły awarii architechture diagram odwołują się do na poniższej ilustracji

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Oto, jak działa awarii:

- Po przez Ciebie nie można Azure, nie zostanie ponownie witryny lokalnej w kilku etapach:
    - **Etap 1**: reprotect maszyny wirtualne Azure, aby rozpoczynały się replikacji wróć do maszyny wirtualne VMware uruchomione w swojej lokalnej witryny. Włączanie reprotection przenosi maszyn wirtualnych w grupie ochrona awarii utworzony automatycznie po utworzeniu pierwotnie grupy ochrony pracy awaryjnej. Po dodaniu grupy ochrony pracy awaryjnej do planu odzyskiwania następnie grupa ochrona awarii również automatycznie został dodany do planu.  Podczas reprotect umożliwia określenie sposobu chce powrócić.
    - **Etap 2**: po maszyny wirtualne usługi Azure są replikacji do swojej witryny lokalnej, należy uruchomić błędu kończy się niepowodzeniem z Azure.
    - **Etap 3**: po danych nie powiodło się ponownie, aby rozpoczynały się replikacji Azure reprotect maszyny wirtualne lokalnych, których nie można wrócić do.

> [AZURE.VIDEO enhanced-vmware-to-azure-failback]


### <a name="failback-to-the-original-or-alternate-location"></a>Aby oryginalny lub alternatywnej lokalizacji

Jeśli nie powiodło się nad maszyny VMware mogą nie ponownie z tym samym źródłem maszyn wirtualnych Jeśli nadal istnieje lokalnego. W tym scenariuszu zmiany różnicy nie powiedzie się ponownie. Należy zauważyć, że:

- Jeśli nie powiodła się nad serwerów fizycznych następnie awarii jest zawsze nowe maszyny VMware.
    - Przed niepowodzeniem ponownie fizyczny komputer należy pamiętać, że
    - Fizyczny komputer chroniony będą powracać dowolną maszyny wirtualnej, gdy nie na powrót z platformy Azure VMware
    - Upewnij się, odnajdywanie co najmniej jedną docelową wzorzec Server wraz z niezbędne hosts ESX-ESXi, do których należy awarii.
- Jeśli nie zostanie ponownie oryginalny maszyn wirtualnych wymagane są następujące:
    - Jeśli maszyn wirtualnych jest zarządzane przez serwer vCenter hosta ESX docelowej wzorzec mają dostęp do magazynu danych maszyny wirtualne.
    - Jeśli maszyn wirtualnych jest na hoście ESX, ale nie jest zarządzane przez vCenter dysk twardy maszyn wirtualnych musi znajdować się w dostępnego magazynu danych przez hosta MT.
    - Jeśli do maszyn wirtualnych jest na hoście ESX, a nie za pomocą vCenter następnie należy wykonać odnajdowania hoście ESX MT przed możesz reprotect. Dotyczy to serwerów ponownie fizycznych w przypadku braku zbyt.
    - Innym rozwiązaniem (jeśli istnieje maszyn wirtualnych lokalnego) jest usunąć przed wykonaniem awarii. Następnie awarii spowoduje to utworzenie nowego maszyn wirtualnych na tym samym hoście jako hosta ESX wzorca docelowego.
    
- Kiedy powrotu do innej lokalizacji dane zostaną przywrócone do samego magazynu danych i tego samego hosta ESX jako używane przez lokalnego serwera docelowej wzorca. 


## <a name="prerequisites"></a>Wymagania wstępne

- Musisz środowisko VMware aby zakończyć się niepowodzeniem wstecz maszyny wirtualne VMware i serwerów fizycznych. Niepowodzenie powrót do serwera fizycznego nie jest obsługiwana.
- Aby powrócić należy utworzono sieć Azure po początkowym skonfigurowaniu ochrony. Awarii wymaga połączenia VPN lub ExpressRoute z Azure sieci, w którym znajdują się w witrynie lokalnej maszyny wirtualne Azure.
- Jeśli maszyny wirtualne, który chcesz ponownie nie są zarządzane przez serwer vCenter, które będą potrzebne, aby upewnić się, masz wymagane uprawnienia do odnajdowania maszyny wirtualne na serwerach vCenter. [Dowiedz się więcej](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
- Jeśli migawki znajdują się na maszyny reprotection wystąpi błąd. Możesz usunąć migawki lub dysków. 
- Zanim nie zostanie ponownie musisz utworzyć wiele składników:
    - **Utwórz serwer proces platformy Azure**. To jest Azure maszyn wirtualnych, które trzeba utworzyć i działanie podczas awarii. Możesz usunąć komputer po zakończeniu awarii.
    - **Utwórz serwer wzorca docelowej**: serwer docelowy wzorca wysyła i odbiera dane awarii. Utworzono lokalnego serwera zarządzania ma zainstalowany domyślnie serwer docelowej wzorca. W zależności od liczby nie powiodło się ruch wstecz może być konieczne do utworzenia serwera osobnych docelowej wzorca dla awarii.
    - Jeśli chcesz utworzyć dodatkowe docelowej wzorca serwerem na Linux, musisz skonfigurować maszyn wirtualnych Linux przed zainstalowaniem serwera wzorca docelowej zgodnie z poniższym opisem.
- Serwer konfiguracji jest wymagane lokalnego po wykonaniu awarii. Podczas awarii maszyny wirtualnej, musi istnieć w bazie danych serwera konfiguracji, Niepowodzenie awarii, które nie zakończyło się pomyślnie. W związku z tym upewnij się, wykonanie regularne zaplanowanej kopii zapasowej serwera. W przypadku awarii należy go przywrócić z tego samego adresu IP, aby umożliwić działanie awarii.

## <a name="set-up-the-process-server-in-azure"></a>Konfigurowanie serwera proces platformy Azure

Należy zainstalować serwer procesów platformy Azure tak, aby maszyny wirtualne Azure można wysłać dane do lokalnego serwera docelowej wzorca. 

1.  W portalu Odzyskiwanie witryny > **Serwerami konfiguracji** wybierz pozycję, aby dodać nowy serwer proces.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)

2.  Określ nazwę serwera proces, a następnie wprowadź nazwę i hasło, które będą używane, aby nawiązać połączenie maszyn wirtualnych Azure jako administrator. W **Konfiguracji serwera** wybierz lokalnego serwera zarządzania i określanie Azure sieci, w którym należy wdrożyć serwer procesów. Należy to sieci, w którym znajdują się maszyny wirtualne Azure. Określ unikatowy adres IP z wybierz pozycję podsieci i zacznij wdrożenia.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

    Zadanie do wdrożenia serwera proces zostanie wyzwolony

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

    Po wdrożeniu serwera proces platformy Azure możesz go przy użyciu poświadczeń określonej zalogować. Podczas pierwszego logowania w oknie dialogowym serwera proces zostanie uruchomiony. Wpisz adres IP lokalnego serwera zarządzania i jej hasło. Pozostaw domyślne ustawienie na porcie 443. Można także pozostawić domyślny port 9443 dla replikacji danych chyba że specjalnie zmodyfikowano to ustawienie podczas konfigurowania lokalnego serwera zarządzania. 

    >[AZURE.NOTE] Serwer nie będą widoczne w obszarze **Właściwości maszyn wirtualnych**. Jest widoczny tylko na karcie **serwer** w management server, które zostały zarejestrowane. Może minąć o 10 do 15 minut na serwerze proces się pojawić.


## <a name="set-up-the-master-target-server-on-premises"></a>Konfigurowanie głównego docelowej lokalnego serwera

Serwer wzorca docelowej odbiera dane awarii. Serwer docelowy wzorca jest instalowane automatycznie na serwerze zarządzania lokalnego, ale jeśli możesz już zakończonych niepowodzeniem ponownie wiele danych może być konieczne konfigurowanie serwera dodatkowe docelowego wzorca. Czynność w następujący sposób:

>[AZURE.NOTE] Jeśli chcesz zainstalować serwer wzorcowej docelowej na Linux, postępuj zgodnie z instrukcjami w następnej procedurze.

1. Jeśli instalujesz serwer docelowy wzorca w systemie Windows, otwórz stronę Szybki Start z maszyn wirtualnych, na którym instalujesz serwer docelowy wzorca i Pobierz plik instalacyjny dla Kreatora konfiguracji Unified Azure witryny odzyskiwania.
2. Uruchom Instalatora i **przed rozpoczęciem** wybierz **Dodaj serwery dodatkowych procesów do skalowania wdrożenia**.
3. Kończenie pracy kreatora w taki sam sposób, kiedy zostało [Konfigurowanie serwera zarządzania](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Na stronie **Szczegóły konfiguracji serwera** określić adres IP tego serwera głównego docelowej i hasło, aby uzyskać dostęp do maszyn wirtualnych.

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Konfigurowanie maszyny Linux jako serwer docelowy wzorca
Aby skonfigurować serwer zarządzania systemem serwera docelowego wzorcowej jako maszyny Linux musisz zainstalować procent) 6.6 S minimalnego systemu operacyjnego Pobierz identyfikatory SCSI dla każdego dysku twardego SCSI, instalowanie pewne dodatkowe pakiety i zastosować niestandardowe zmiany.

#### <a name="install-centos-66"></a>Instalowanie CentOS 6.6

1.  Zainstaluj system operacyjny z minimalnymi CentOS 6.6 na serwerze zarządzania maszyn wirtualnych. Zachowaj ISO stacji dysków DVD i uruchom komputer. Pominąć badania multimediów, zaznacz US języka angielskiego na język, wybierz **Podstawowe urządzenia miejsca do magazynowania**, sprawdź, czy dysk twardy nie mają żadnych ważnych danych i kliknij przycisk **Tak**, odrzucenie danych. Wprowadź nazwę hosta serwera zarządzania i wybierz kartę serwer.  W oknie dialogowym **Edytowanie systemu** zaznacz pole wyboru** Połącz automatycznie** i Dodaj statyczny adres IP, sieci i ustawienia DNS. Określ strefę czasową i hasła głównego w celu uzyskania dostępu do serwera zarządzania. 
2.  Monit żądany typ instalacji chcesz, wybierz pozycję **Utwórz układ niestandardowy** partycją. Po kliknięciu przycisku **Dalej** **Wybierz pozycję** i kliknij przycisk Utwórz. Tworzenie **/**, **/var/crash** i **/ home partycje** z **FS typ:** **ext4**. Tworzenie partion wymiany jako **FS typ: wymiany**.
3.  Jeśli istniejącego urządzenia znajdują się, że zostanie wyświetlony komunikat ostrzegawczy. Kliknij **Format** sformatowanie z ustawieniami partycją. Kliknij przycisk **zapisać zmiany na dysku** , aby zastosować zmiany partycją.
4.  Wybierz **Moduł ładujący uruchamiania instalacji** > **następnego** zainstalować moduł ładujący uruchamiania na partycją katalogu głównego.
5.  Po zakończeniu instalacji kliknij przycisk **Uruchom ponownie**.


#### <a name="retrieve-the-scsi-ids"></a>Pobierz identyfikatory SCSI

1. Po zakończeniu instalacji Pobierz identyfikatory SCSI dla każdego dysku twardego SCSI w maszyn wirtualnych. Aby wykonać ten Zamknij serwer zarządzania maszyn wirtualnych, we właściwościach maszyn wirtualnych w VMware kliknij prawym przyciskiem myszy pozycję maszyn wirtualnych > **Edytuj ustawienia** > **Opcje**.
2. Wybierz pozycję **Zaawansowane** > **Ogólne elementu** i kliknij przycisk **Parametry konfiguracji**. Ta opcja jest de-active, gdy komputer jest uruchomiony. Aby go uaktywnić należy zamknąć komputera.
3. Jeśli wiersz **dysku. EnableUUID** istnieje upewnij się, wartość jest ustawiona na **PRAWDA** (jest uwzględniana wielkość liter). Jeśli jest już możesz anulować i po jego uruchomieniu przetestować polecenie SCSI w systemie operacyjnym gościa. 
4.  Jeśli wiersz nie istniejących kliknij przycisk **Dodaj wiersz** — i dodaj go na wartość **PRAWDA** . Nie używaj podwójnych cudzysłowów.

#### <a name="install-additional-packages"></a>Instalowanie dodatkowe pakiety

Musisz pobrać i zainstalować niektóre dodatkowe pakiety. 

1.  Upewnij się, że serwer docelowy wzorca jest połączony z Internetem.
2.  Polecenie, aby pobrać i zainstalować pakiety 15 z repozytorium CentOS: **# yum instalowanie — y xfsprogs perl lsscsi rsync wget kexec narzędzi**.
3.  Jeśli na komputerach źródła jest ochrona systemem Linux wit Reiser lub XFS pliku systemu głównego lub uruchamiania urządzenia, a następnie należy pobrać i zainstalować dodatkowe pakiety w następujący sposób:

    - # <a name="cd-usrlocal"></a>dysk CD /usr/local
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
    - # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>reiserfs kmod-reiserfs 0,0 1.el6.elrepo.x86_64.rpm — ivh obrotów na minutę-witryny-3.6.21-1.el6.elrepo.x86_64.rpm
    - # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
    - # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>obrotów na minutę — ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Stosowanie niestandardowych zmian

Wykonaj następujące czynności, aby zastosować zmiany niestandardowe po wprowadzeniu wykonaj kroki po instalacji i zainstalowaniu opakowań:

1.  Kopiowanie RHEL 6-64 Unified agentem binarne do maszyn wirtualnych. Uruchom polecenie untar pliku binarnego: **tar — zxvf <file name> **
2.  Polecenie, aby nadać uprawnienia: **# chmod 755./ApplyCustomChanges.sh**
3.  Uruchom skrypt: **#./ApplyCustomChanges.sh**. Skrypt należy uruchomić tylko raz. Po pomyślnym uruchomieniem skryptu, należy ponownie uruchomić serwer.


## <a name="run-the-failback"></a>Uruchamianie awarii

### <a name="reprotect-the-azure-vms"></a>Reprotect Azure maszyny wirtualne

1.  W portalu Odzyskiwanie witryny > karta **maszyn** wybierz maszyn wirtualnych jest powiodła się na, a następnie kliknij pozycję **ponownie ochronę**.
2.  W **Serwer docelowej główny** i **Serwer procesów** wybierz lokalnego serwera docelowej wzorca i serwer proces maszyn wirtualnych Azure.
3.  Wybierz konto, dla którego skonfigurowano do łączenia się z maszyn wirtualnych.
4.  Wybierz wersję awarii grupa ochrona. Na przykład jeśli maszyn wirtualnych jest chroniony w PG1 możesz najpierw wybierz PG1_Failback.
5.  Jeśli chcesz odzyskać do innej lokalizacji, wybierz dysk przechowywania i skonfigurowane dla serwera docelowego wzorca magazynu danych. Kiedy nie zostanie powrót do lokalnej witryny maszyny wirtualne VMware w planie ochrony awarii użyje samego magazynu danych jako serwera głównego docelowego. Jeśli chcesz odzyskać replice Azure maszyn wirtualnych do tego samego lokalnych maszyn wirtualnych, a następnie maszyn wirtualnych lokalnego powinna już być w tym samym magazynu danych jako serwer docelowy wzorca. Jeśli istnieje nie maszyn wirtualnych lokalnego nową zostanie utworzony podczas reprotection.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)

6.  Po kliknięciu przycisku **OK** , aby rozpocząć reprotection zadanie rozpoczyna się replikacji maszyn wirtualnych z Azure do lokalnej witryny. Na karcie **zadania** możesz śledzić postęp.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Uruchom trybie awaryjnym do lokalnej witryny

Po reprotection maszyn wirtualnych jest przenoszony do wersji awarii grupy ochrony i są automatycznie dodawane do planu odzyskiwania, którego użyto do przełączania awaryjnego Azure, jeśli istnieje.

1.  Na stronie **Plany odzyskiwania** wybierz plan odzyskiwania zawierający grupę awarii, a następnie kliknij pozycję **pracy awaryjnej** > **Nieplanowanego przełączania awaryjnego**.
2.  W **Pracy awaryjnej potwierdzenia** Sprawdź pracy awaryjnej kierunek (Azure) i wybierz punkt odzyskiwania, który ma być używany do przełączania awaryjnego (Najnowsza wersja). W przypadku włączenia **Maszyn wirtualnych wielokrotne** podczas konfigurowania właściwości replikacji można odzyskać najnowszą wersję aplikacji lub punkt odzyskiwania spójne awarii. Kliknij znacznik wyboru, aby rozpocząć tym przełączeniu.
3.  Podczas pracy awaryjnej Odzyskiwanie witryny zostanie zamknięty maszyny wirtualne Azure. Po sprawdzeniu, że wykonano tej awarii zgodnie z oczekiwaniami, możesz można możesz sprawdzić maszyny wirtualne Azure mają został zamknięty zgodnie z oczekiwaniami.

### <a name="reprotect-the--on-premises-site"></a>Reprotect lokalnej witryny

Po zakończeniu awarii danych będzie ponownie w witrynie lokalnej, ale nie będą one chronione. Aby rozpocząć replikacji Azure ponownie wykonaj następujące czynności:

1.  W portalu Odzyskiwanie witryny > Wybierz kartę **maszyn** maszyny wirtualne, które nie mają kopii i kliknij polecenie **Chroń ponownie**. 
2.  Upewnij się, że replikacja Azure działa zgodnie z oczekiwaniami, platformy Azure możesz usunąć maszyny wirtualne Azure (obecnie nie działa), które nie zostały ponownie.


### <a name="common-issues-in-failback"></a>Typowe problemy w awarii

1. Jeśli odnajdywanie vCenter użytkownika tylko do odczytu i ochrona maszyn wirtualnych skutku i pracy awaryjnej działa. W momencie Reprotect go zakończy się niepowodzeniem, ponieważ nie można odnaleźć datastores. Jako objawem nie zobaczysz datastores ponownie chroniąc na liście. Aby rozwiązać ten problem, można zaktualizować poświadczeń vCenter odpowiednie konto, które ma uprawnienia i spróbuj ponownie to zadanie. [Dowiedz się więcej](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Gdy awarii maszyny Linux i uruchom go lokalna, zobaczysz, że odinstalowaniu pakietu Menedżer sieci z komputera. Jest to spowodowane maszyn wirtualnych jest odzyskać platformy Azure, Menedżera sieciowego pakietu usunięcie.
3. Gdy maszyny skonfigurowano statyczny adres IP i to nie powiodło się nad Azure, adres IP zostanie pobrana przez DHCP. Gdy po wymuszeniu przejęcia Wstecz, aby lokalna, maszyn wirtualnych w dalszym ciągu używa DHCP umożliwia uzyskanie adresu IP. Będzie konieczne ręczne logowania do komputera i ustawianie adresu IP z powrotem do statycznego adresu w razie potrzeby.
4. Jeśli używasz bezpłatna wersja ESXi 5.5 lub vSphere 6 monitor maszyny wirtualnej bezpłatna wersja powiedzie się pracy awaryjnej, ale nie powiedzie się awarii. Konieczne będzie ned przeprowadzić uaktualnienie do albo licencji oceny, aby włączyć awarii.

## <a name="failing-back-with-expressroute"></a>Awarie wstecz z ExpressRoute

Ponownie przez połączenie VPN ani Azure ExpressRoute może się nie powieść. Jeśli chcesz użyć notatki ExpressRoute następujące czynności:

- ExpressRoute należy konfigurowanie na Azure wirtualnej sieci, których źródłem błędów komputerach przez, w których maszyny wirtualne Azure znajdują się po awaria wystąpiła.
- Dane są replikowane na konto Azure miejsca do magazynowania w publicznej punktu końcowego. Należy skonfigurować publicznej zaglądanie w ExpressRoute z centrum danych docelowych replikacji Odzyskiwanie witryny za pomocą ExpressRoute.



