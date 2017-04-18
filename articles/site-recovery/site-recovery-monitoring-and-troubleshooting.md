<properties
    pageTitle="Monitorowanie i rozwiązywanie problemów z ochroną dla maszyn wirtualnych i serwerów fizycznych | Auzre firmy Microsoft" 
    description="Odzyskiwanie witryny Azure współrzędne replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych znajdujących się na serwerach lokalnego Azure lub pomocnicza centrum danych. W tym artykule umożliwia monitorowanie i rozwiązywanie problemów z ochroną VMM lub witrynie funkcji Hyper-V." 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/13/2016"    
    ms.author="rajanaki"/>
    
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Monitorowanie i rozwiązywanie problemów z ochroną dla maszyn wirtualnych i serwerów fizycznych

Ten monitorowania i rozwiązywania problemów z przewodnika umożliwia informacje śledzenia stanu replikacji i rozwiązywanie problemów dotyczących technik Odzyskiwanie witryny Azure.

## <a name="understanding-the-components"></a>Opis składników

### <a name="vmwarephysical-site-deployment-for-replication-between-on-premises-and-azure"></a>Wdrażanie witryny VMware/fizycznej replikacji między wdrożeniem lokalnym a Azure.
Konfigurowanie DR między lokalnego VMware/fizycznej komputera; Serwer konfiguracji, docelowej wzorca i proces serwera musi skonfigurować. Podczas włączania ochrony z serwerem źródłowym Azure Odzyskiwanie witryny zostaną zainstalowane usługi mobilności. Publikowanie awarii lokalnego po na nie powiedzie się z serwerem źródłowym Azure, trzeba klientów konfiguracji serwera proces w Azure i docelowej wzorzec lokalnego serwera ochrony z serwerem źródłowym powrót do skompilowane lokalnego. 

![Wdrażanie witryny VMware/fizycznej replikacji między lokalnego & Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises-site"></a>Wdrażanie witryny VMM replikacji między lokalnej witryny.

Podczas konfigurowania DR między dwoma lokalnego lokalizacji. Dostawca odzyskiwania Azure witryny musi pobrać i zainstalować na serwerze VMM. Dostawca musi łączność z Internetem aby upewnić się, że wszystkie operacje określone przez Azure Portal otrzymuje przetłumaczony operacje lokalnego jak włączyć ochronę, zamknięcie strony podstawowego maszyn wirtualnych jako część pracy awaryjnej itp.

![Wdrażanie witryny VMM replikacji między lokalnej witryny](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="vmm-site-deployment-for-replication-between-on-premises--azure"></a>Wdrażanie witryny VMM replikacji między lokalnego & Azure.

Podczas konfigurowania DR między lokalnego & Azure; Dostawca odzyskiwania Azure witryny musi pobrać i zainstalować na serwerze VMM wraz z agenta usługi Azure odzyskiwania, które mają zostać zainstalowane na każdym hoście funkcji Hyper-V. Aby uzyskać więcej informacji można znaleźć [Opis witryny do ochrony Azure](./site-recovery-understanding-site-to-azure-protection.md) .

![Wdrażanie witryny VMM replikacji między lokalnego & Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises--azure"></a>Wdrażanie witryny funkcji Hyper-V replikacji między lokalnego & Azure

To jest identyczny wdrożenia VMM — tylko różnica jest dostawcy i agenta otrzymuje zainstalowanym host funkcji Hyper-V. Aby uzyskać więcej informacji można znaleźć [Opis witryny do ochrony Azure](./site-recovery-understanding-site-to-azure-protection.md) .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Monitorowanie operacji konfiguracji, ochrony i odzyskiwania

Każdej operacji w automatycznego odzyskiwania systemu otrzymuje inspekcji i są śledzone w obszarze karty "Zadania". W przypadku dowolnej konfiguracji, ochrony lub błędu odzyskiwania przejdź do karty zadania i sprawdzić, czy są wszystkie błędy.

![Monitorowanie operacji konfiguracji, ochrony i odzyskiwania](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Po znalezieniu błędy w obszarze Widok zadania wybierz zadanie i kliknij pozycję Szczegóły błędu dla tego zadania.

![Monitorowanie operacji konfiguracji, ochrony i odzyskiwania](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Szczegóły błędu pomoże zidentyfikować możliwe przyczyny i zalecenia dotyczące problemu.

![Monitorowanie operacji konfiguracji, ochrony i odzyskiwania](media/site-recovery-monitoring-and-troubleshooting/image5.png)

W powyższym przypadku wydaje się inna operacja, która jest w toku ze względu na które kończy się niepowodzeniem konfiguracji ochrony. Upewnij się, rozwiązać ten problem, zgodnie z rekomendacji — dostępne po kliknij RESART, aby ponownie rozpocząć operację.

![Monitorowanie operacji konfiguracji, ochrony i odzyskiwania](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Opcja Uruchom ponownie nie jest dostępna dla wszystkich działań — tych, które nie zawiera opcji Uruchom ponownie przejść z powrotem do obiektu i wykonaj ponownie operację ponownie. Każdego zadania można anulować w dowolnym momencie jednoczesnym za pomocą przycisku Anuluj w toku.

![Monitorowanie operacji konfiguracji, ochrony i odzyskiwania](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machine"></a>Monitorowanie kondycji replikacji dla maszyn wirtualnych

Dostawca automatycznego odzyskiwania systemu centralnej i zdalne monitorowanie przez Azure Portal dla każdego z chronionego podmiotów. Przejdź do chronionych elementów udzielenia po wybierz chmury VMM lub grup ochrony. Karta chmury VMM jest tylko w przypadku wdrożeń VMM podstawie i wszystkich innych sytuacjach mieć chronionego oddziały na karcie ochrona grupy.

![Monitorowanie kondycji replikacji dla maszyn wirtualnych](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Dostępne po wybierz jednostkę chronionych w obszarze odpowiednich chmury lub grupa ochrona. Po wybraniu wszystkich jednostek chronionego dozwolone operacje są wyświetlane w dolnym okienku.

![Monitorowanie kondycji replikacji dla maszyn wirtualnych](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Jak pokazano powyżej w przypadku maszyny wirtualnej, że kondycji jest krytyczne — możesz kliknij przycisk Szczegóły błędu w dół, aby wyświetlić błąd. Oparte na "możliwe przyczyny" i "Zalecenie" wymienionych rozwiązać ten problem.

![Monitorowanie kondycji replikacji dla maszyn wirtualnych](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Monitorowanie kondycji replikacji dla maszyn wirtualnych](media/site-recovery-monitoring-and-troubleshooting/image11.png)

Uwaga: W przypadku dowolnej aktywnej operacji, które są w toku lub zakończonego niepowodzeniem następnie przejdź do widoku zadań jak wspomniano wcześniej, aby wyświetlić zadania konkretnego problemu.

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Rozwiązywanie problemów funkcji Hyper-V lokalnego

Nawiązywanie połączenia z konsoli Menedżera funkcji Hyper-V lokalnego zaznacz maszyny wirtualnej i wyświetlić stanu replikacji.

![Rozwiązywanie problemów funkcji Hyper-V lokalnego](media/site-recovery-monitoring-and-troubleshooting/image12.png)

W tym przypadku *Stanu replikacji* jest jest oznaczony jako krytyczne — *Stanu replikacji w widoku* , aby wyświetlić szczegóły.

![Rozwiązywanie problemów funkcji Hyper-V lokalnego](media/site-recovery-monitoring-and-troubleshooting/image13.png)

W przypadku gdy replikacji została wstrzymana dla maszyny wirtualnej, kliknij prawym przyciskiem myszy wybierz *replikacji*->*replikacji Życiorys*
![Rozwiązywanie problemów z funkcji Hyper-V lokalnego](media/site-recovery-monitoring-and-troubleshooting/image19.png)

W przypadku maszyn wirtualnych migruje nowy funkcji Hyper-V host (w grupie lub komputerze autonomicznej), który został skonfigurowany do automatycznego odzyskiwania systemu, nie dotyczy replikacji dla maszyny wirtualnej. Zapewnić spełniają wszystkie na — wymagania nowego hosta funkcji Hyper-V i jest skonfigurowany przy użyciu automatycznego odzyskiwania systemu.

### <a name="event-log"></a>Dziennik zdarzeń

| Źródła zdarzeń                | Szczegóły                                                                                                                                                                                           |
|-------------------------  |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| **Aplikacje i usługi Dzienniki/Microsoft/VirtualMachineManager /-administrator serwera** (VMM Server)   |  Dzięki temu rejestrowanie przydatne dla Rozwiązywanie problemów z wielu różnych VMM. |
| **Aplikacje i usługi Dzienniki MicrosoftAzureRecoveryServices replikacji** (Funkcji hyper-V hosta)   | Dzięki temu rejestrowanie przydatne dla Rozwiązywanie problemów z wielu agenta usługi Microsoft Azure odzyskiwania. <br/> ![Źródło zdarzenia dla hosta funkcji Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Aplikacje i usługi witryny z dzienników/Microsoft/Azure odzyskiwania/dostawcy operacyjne** (Funkcji hyper-V hosta)   | Dzięki temu rejestrowanie przydatne dla Rozwiązywanie problemów z wielu Usługa odzyskiwania systemu Microsoft Azure witryny. <br/> ![Źródło zdarzenia dla hosta funkcji Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Aplikacje i dzienniki/Microsoft/systemu Windows i funkcji Hyper-V-VMMS-administrator usługi** (Funkcji hyper-V hosta) | Dzięki temu rejestrowanie przydatne dla rozwiązywanie wielu problemów zarządzania maszyn wirtualnych funkcji Hyper-V. <br/> ![Źródło zdarzenia dla hosta funkcji Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |


### <a name="hyper-v-replication-logging-options"></a>Opcje rejestrowania replikacji funkcji Hyper-V

W funkcji Hyper-V-VMMS są rejestrowane wszystkie zdarzenia dotyczące funkcji Hyper-V replice\\administrator dziennika znajduje się w obszarze **Dzienniki aplikacji i usług\\Microsoft\\Windows**. Ponadto analityczny dziennika mogą mieć dostęp do funkcji Hyper-V-VMMS. Aby włączyć ten dziennik, najpierw upewnij dzienniki analityczne i debugowania są widoczne w Podglądzie zdarzeń. Następnie otwórz Podgląd zdarzeń, w **menu Widok**, kliknij przycisk **Pokaż analityczne i dzienniki debugowania**.

![Rozwiązywanie problemów funkcji Hyper-V lokalnego](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Analitycznym dziennika jest widoczny w funkcji Hyper-V-VMMS

![Rozwiązywanie problemów funkcji Hyper-V lokalnego](media/site-recovery-monitoring-and-troubleshooting/image15.png)

W okienku **akcji** kliknij **Włącz dziennik**. Po włączeniu wydaje się w **Monitorze wydajności** podczas sesji śledzenia zdarzeń znajduje się w obszarze **Zestawy modułów zbierających dane.**

![Rozwiązywanie problemów funkcji Hyper-V lokalnego](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Aby wyświetlić informacje zbierane, najpierw Zatrzymaj sesję śledzenia przez wyłączenie dziennika, a następnie zapisać dziennik i ponownie otworzyć w Podglądzie zdarzeń lub korzystać z innych narzędzi przekonwertować ją w razie potrzeby.



## <a name="reaching-out-for-microsoft-support"></a>Możliwość kontaktowania się z for Microsoft Support

### <a name="log-collection"></a>Zbieranie dzienników

Ochrona VMM witryn można znaleźć [Zbieranie dzienników automatycznego odzyskiwania systemu przy użyciu narzędzia do pomocy technicznej diagnostyki platformy (SDP)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) na potrzeby zbierania dzienników wymagane.

Ochrony witryny funkcji Hyper-V pobrać [Narzędzie](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) i wykonywanie go na hoście funkcji Hyper-V na potrzeby zbierania dzienników.

Scenariusze VMware/fizycznej można znaleźć w [Zbiorze dziennika odzyskiwania Azure witryn, VMware i fizycznej ochrony witryny](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) na potrzeby zbierania dzienników wymagane.

Narzędzie gromadzi dzienniki lokalnie w obszarze losowo nazwanym podfolderze w obszarze **%LocalAppData%\ElevatedDiagnostics**

![Przykładowe kroki przedstawione z ochrony witryny funkcji Hyper-V.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="opening-a-support-ticket"></a>Otwieranie bilet pomocy technicznej

Aby podnieść bilet pomocy technicznej automatycznego odzyskiwania systemu, docieranie do obsługi Azure za pomocą adresu URL w <http://aka.ms/getazuresupport>

## <a name="kb-articles"></a>Artykuły z bazy wiedzy

-   [Jak zachować literę chronionego maszyn wirtualnych, które nie powiodło się nad lub migracji Azure](http://support.microsoft.com/kb/3031135)
-   [Jak zarządzać lokalnego do wykorzystania przepustowości sieci ochrony Azure](https://support.microsoft.com/kb/3056159)
-   [Automatycznego odzyskiwania systemu: "nie można znaleźć zasobu klaster" błąd podczas próby włączenia ochrony dla maszyny wirtualnej](http://support.microsoft.com/kb/3010979)
-   [Opis i rozwiązywanie problemów z przewodnika replice funkcji Hyper-V](http://www.microsoft.com/en-in/download/details.aspx?id=29016) 

## <a name="common-asr-errors-and-their-resolutions"></a>Typowe błędy automatycznego odzyskiwania systemu i ich rozwiązania

Poniżej przedstawiono typowe błędy, które mogą trafień oraz ich rozwiązania. Każdy błąd jest opisane w osobnej stronie typu WIKI.

### <a name="general"></a>Ogólne
-   <span style="color:green;">Nowy</span> Zadania [niepowodzeniem z powodu błędu "operacja jest w toku". Błąd 505, 514, 532](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
-   <span style="color:green;">Nowy</span> Zadania [awarii z powodu błędu "Serwer nie jest połączony z Internetem". Błąd 25018](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Konfiguracja
-   [Nie można zarejestrować na serwerze VMM ze względu na błąd wewnętrzny. Zajrzyj do widoku zadań w portalu odzyskiwania witryny, aby uzyskać więcej informacji o błędzie. Uruchom Instalatora ponownie, aby zarejestrować serwer.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
-   [Nie można połączyć do magazynu Menedżer odzyskiwania funkcji Hyper-V. Sprawdź ustawienia serwera proxy i spróbuj ponownie później.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfiguracja
-   [Nie można utworzyć grupy ochrony: Wystąpił błąd podczas pobierania listy serwerów.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
-   [Klaster hosta funkcji Hyper-V zawiera co najmniej jeden statyczne sieciową lub nie połączonego karty są skonfigurowane do używania DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
-   [VMM nie ma uprawnień do wykonania akcji](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
-   [Nie można wybrać konta miejsca do magazynowania w subskrypcji podczas konfigurowania ochrony](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Ochrona
- <span style="color:green;">Nowy</span> Niepowodzenie [Włącz ochronę z błędu "ochrona nie można skonfigurować dla maszyny wirtualnej". Błąd 60007, 40003](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
- <span style="color:green;">Nowy</span> Niepowodzenie [Włącz ochronę z błąd "nie może być włączona ochrona maszyny wirtualnej." Błąd 70094](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
- <span style="color:green;">Nowy</span> Błąd migracja [23848 - maszyny wirtualnej ma zostać przeniesione przy użyciu typu Live. To może spowodować uszkodzenie stan ochrony zwrotu maszyny wirtualnej.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx) 
- [Włącz ochronę nie powiodło się, ponieważ Agent nie jest zainstalowany na komputerze obsługującym](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
- [Nie można odnaleźć odpowiednie hosta maszyny wirtualnej replice - ze względu na niskie obliczyć zasobów](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
- [Nie można znaleźć odpowiedniego hosta maszyny wirtualnej replice - ze względu na nie logiczne sieciowej](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
- [Nie można połączyć z komputerem hosta replice — nie można nawiązać połączenia](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)


### <a name="recovery"></a>Odzyskiwanie
- VMM nie można ukończyć tej operacji hosta-
    -   [Nie na punkt odzyskiwania zaznaczonego maszyn wirtualnych: ogólny błąd odmowy dostępu.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
    -   [Funkcji Hyper-V nie można przechodzić do wybranego punkt dla maszyn wirtualnych: Operacja przerwana spróbuj nowszą punkt odzyskiwania. (0x80004004)](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
    -   Nie można połączenia z serwerem wskazanych (0x00002EFD)
        -   [Nie można włączyć odwrotnej replikacji dla maszyny wirtualnej funkcji Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
        -   [Nie można włączyć replikacji maszyn wirtualnych maszyn wirtualnych funkcji Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
    -   [Nie można przekazać awaryjnego maszyn wirtualnych](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
-   [Plan odzyskiwania zawiera maszyn wirtualnych, które nie są gotowe do planowanego przełączania awaryjnego](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
-   [Maszyna wirtualna nie jest gotowa do planowanego przełączania awaryjnego](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   [Maszyn wirtualnych nie działa i nie jest wyłączony](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
-   [W nowym oknie operacji pasek stało na maszyn wirtualnych i zatwierdź pracy awaryjnej nie powiodła się](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
-   Testowanie pracy awaryjnej
    -   [Nie można zainicjować pracy awaryjnej, ponieważ test pracy awaryjnej jest w toku](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
-   <span style="color:green;">Nowy</span>  Pracy awaryjnej przekroczenie limitu czasu słowem "task PreFailoverWorkflow WaitForScriptExecutionTaskTimeout" ze względu na ustawienia konfiguracji skojarzone z maszyny wirtualnej lub podsieci, do którego komputer należy do grupy zabezpieczeń sieci. Zapoznaj się z ["PreFailoverWorkflow zadania WaitForScriptExecutionTaskTimeout"](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) , aby uzyskać szczegółowe informacje.


### <a name="configuration-server-process-server-master-target"></a>Cel wzorzec serwera, serwer proces konfiguracji
Konfiguracja serwer (CS), proces (PS) Targer wzorzec (MT)
-   [Host ESXi obsługiwana jako maszyny PS-ks zakończy się niepowodzeniem z fioletowy ekran.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Rozwiązywanie problemów po przełączeniu pulpitu zdalnego
-   Wielu klientów mają wypadek problemy, aby nawiązać połączenie nie powiodło się przez maszyn wirtualnych w Azure. [Użyj rozwiązywania problemów z dokumentu, aby RDP do maszyn wirtualnych](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Dodawanie publiczny adres IP na komputerze wirtualnych Menedżera zasobów
Jeśli przycisk **Połącz** w portalu jest wyszarzona, a nie nawiązano Azure za pośrednictwem rozsyłania Express lub połączenie VPN witryny do witryny, musisz utworzyć i przypisać do maszyn wirtualnych publiczny adres IP, zanim będzie można używać RDP-SSH. Wykonaj poniższe czynności, aby dodać publiczny adres IP na karcie sieciowej maszyny wirtualnej.  

![Dodawanie publiczny adres IP dla interfejsu sieci nie powiodło się nad maszyn wirtualnych](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)