<properties
    pageTitle="Powielić maszyn wirtualnych VMware i serwerów fizycznych Azure z Odzyskiwanie witryny Azure (starsza wersja) | Microsoft Azure" 
    description="Opisano, jak odtworzyć pośrednictwem lokalnych SMS i systemu Windows i Linux oraz serwerów fizycznych Azure za pomocą Odzyskiwanie witryny Azure we wdrożeniu starsze w portalu klasyczny." 
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>Powielić maszyn wirtualnych VMware i serwerów fizycznych Azure z Odzyskiwanie witryny Azure za pomocą portalu klasyczny (starsza wersja)

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmware-to-azure.md)
- [Klasyczny portalu](site-recovery-vmware-to-azure-classic.md)
- [Klasyczny portalu (starsza wersja)](site-recovery-vmware-to-azure-classic-legacy.md)


Odzyskiwanie Azure witryny — Zapraszamy! W tym artykule opisano starsze wdrożenia replikacji VMware w lokalnym środowisku maszyn wirtualnych systemu lub systemu Windows i Linux oraz serwerów fizycznych Azure za pomocą Odzyskiwanie witryny Azure w portalu klasyczny.

## <a name="overview"></a>Omówienie

Organizacje muszą strategii BCDR, która określa sposób aplikacje, obciążenia i danych utrzymywania działa i jest dostępny podczas zaplanowane i niezaplanowane przestoje i odzyskiwanie do normalnych warunkach pracy tak szybko, jak to możliwe. Strategii BCDR należy zachować dane biznesowe, bezpiecznych i odzyskania i upewnij się, obciążenia pozostają nieprzerwanie dostępne, gdy wystąpi awarii.

Odzyskiwanie witryny jest usługą Azure, która pozwala strategii BCDR przez orchestrating replikacji lokalnych serwerów fizycznych i maszyn wirtualnych w chmurze (Azure) lub pomocnicza centrum danych. W przypadku wystąpienia awarii w lokalizacji podstawowego nie na pomocniczym lokalizacji, aby aplikacje i obciążenia dostępne. Możesz zakończyć się niepowodzeniem powrót do swojej lokalizacji podstawowej podczas powraca do normalnego działania. Dowiedz się więcej na [Co to jest Azure witryny odzyskiwanie?](site-recovery-overview.md)


>[AZURE.WARNING] Ten artykuł zawiera **instrukcje starsze**. Nie używaj go w przypadku wdrożeń nowy. Zamiast tego [Wykonaj te instrukcje](site-recovery-vmware-to-azure.md) , aby wdrożyć Odzyskiwanie witryny w Azure portal lub [Korzystając z tych instrukcji](site-recovery-vmware-to-azure-classic.md) , aby skonfigurować rozszerzone wdrożenia w portalu klasyczny. Jeśli zostały już rozmieszczone przy użyciu metody opisane w tym artykule, zalecamy możesz przeprowadzić migrację do wdrożenia rozszerzone w portalu klasyczny.


## <a name="migrate-to-the-enhanced-deployment"></a>Migrowanie do wdrożenia rozszerzone

W tej sekcji ma zastosowanie tylko jeśli zostały już rozmieszczone Odzyskiwanie witryny zgodnie z instrukcjami zawartymi w tym artykule.

Aby przeprowadzić migrację istniejącej wdrożenia trzeba będzie:

1. Wdrażanie nowe składniki Odzyskiwanie witryny w witrynie lokalnej.
2. Skonfiguruj poświadczenia automatycznego wykrywania VMware maszyny wirtualne na nowy serwer konfiguracji.
3. Dowiedz się z serwerami VMware z nowym serwerem konfiguracji.
3. Tworzenie nowej grupy ochrony z nowym serwerem konfiguracji.


Przed rozpoczęciem pracy:

- Zaleca się konfigurowanie oknem konserwacji migracji.
- Opcja **Migrowanie maszyn** jest dostępne tylko wtedy, gdy istniejących grup ochrony, które zostały utworzone podczas wdrażania starsze.
- Po wykonaniu kroków migracji może zająć 15 minut lub więcej odświeżyć poświadczeń i aby odnaleźć i odświeżanie maszyn wirtualnych tak, aby dodać je do grupy ochrony. Można ręcznie odświeżyć zamiast oczekiwania. 

Migrowanie w następujący sposób:

1. Przeczytaj o [rozszerzonym wdrożenia w portalu klasyczny](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Przejrzyj rozszerzone [Architektura](site-recovery-vmware-to-azure-classic.md#scenario-architecture)i [wymagania wstępne](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
2. Odinstaluj usługę mobilności z komputerów, które są obecnie replikacji. Nową wersję usługi zostanie zainstalowany na komputerach, po dodaniu ich do nowej grupy ochrony.
3. Pobierz [klucz rejestru magazynu](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) i [uruchomić Kreatora konfiguracji ujednolicony](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) , aby zainstalować serwer konfiguracji serwera proces i składniki serwera głównego docelowej. Dowiedz się więcej na temat [planowania pojemności](site-recovery-vmware-to-azure-classic.md#capacity-planning).
4. [Skonfiguruj poświadczenia](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) odzyskiwanie tej witryny służy do dostępu do serwera VMware w celu automatycznego wykrywania maszyny wirtualne VMware. Informacje na temat [wymagane uprawnienia](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. Dodaj [Serwery vCenter lub vSphere hosts](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts). Może potrwać 15 minut, aby uzyskać więcej informacji na serwerów są wyświetlane w portalu Odzyskiwanie witryny.
6. Tworzenie [nowej grupy ochrony](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group). Może potrwać do 15 minut portalu Odśwież, aby maszyn wirtualnych zostaną wykryte i są wyświetlane. Jeśli nie chcesz czekać można wyróżnić nazwę serwera zarządzania (nie klikaj jej) > **Odśwież**.
7. W nowej grupie ochrona kliknij przycisk **Migrację komputerów**.

    ![Dodawanie konta](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)

8. W **Środowisku maszyn wybierz** zaznacz grupa ochrona, który chcesz przeprowadzić migrację z i komputerach, który chcesz przeprowadzić migrację.

    ![Dodawanie konta](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)

9. W przypadku **Konfigurowania ustawień docelowej** Określ, czy te same ustawienia dla wszystkich komputerów i wybierz serwer procesów i konto Azure miejsca do magazynowania. Jeśli nie masz serwer oddzielnego procesu są to adres IP serwera konfiguracji.


    ![Dodawanie konta](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migracja kont miejsca do magazynowania](../resource-group-move-resources.md) w grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane w przypadku kont miejsca do magazynowania na potrzeby wdrażania Odzyskiwanie witryny.

10. **Określanie konta**wybierz konto, utworzonej na serwerze proces dostęp do komputera, aby przekazać nowej wersji usługi mobilności.

    ![Dodawanie konta](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)

11. Odzyskiwanie witryny będą migrację zreplikowanej danych do podanego konta magazynu platformy Azure. Opcjonalnie można ponownie użyć konta miejsca do magazynowania, którego użyto w starszych wdrożenia.
12. Po zakończeniu zadania automatycznie zsynchronizuje maszyn wirtualnych. Po zakończeniu synchronizacji maszyn wirtualnych można usunąć z grupy starszych ochrony.
13. Po migracji wszystkich komputerach można usunąć grupa starszych ochrona.
14. Pamiętaj, aby określić właściwości pracy awaryjnej dla urządzeń i ustawień sieci Azure po zakończeniu synchronizacji.
15. Jeśli masz istniejące plany odzyskiwania, można je migrować do rozszerzonego wdrażania z opcją **Migracji planowanie odzyskiwania** . Tylko należy to zrobić, po przeprowadzeniu migracji wszystkich komputerach chroniony. 

    ![Dodawanie konta](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

>[AZURE.NOTE] Po zakończeniu migracji kontynuować [artykuł rozszerzone](site-recovery-vmware-to-azure-classic.md). Pozostałej części tego artykułu starsze nie będzie już odpowiednie i nie musisz wykonywać dalszych czynności opisane w it **.




## <a name="what-do-i-need"></a>Co jest potrzebne?

Ten diagram przedstawia składniki wdrażania.

![Nowe magazynu](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Oto, czego potrzebujesz:

**Składnik** | **Wdrożenie** | **Szczegóły**
--- | --- | ---
**Serwer konfiguracji** | Azure standardowy A3 maszyny wirtualnej w tej samej subskrypcji jako Odzyskiwanie witryny. | Serwer konfiguracji współrzędne komunikacji między chronionego maszyn, proces serwera i serwerów docelowych wzorca w Azure. Konfiguruje replikacji i odzyskiwania współrzędne w Azure gdy wystąpi awaryjne przeniesienie.
**Serwer docelowy wzorca** | Azure maszyn wirtualnych — albo systemu Windows server na podstawie Galeria Windows Server 2012 R2 (w celu ochrony komputerów systemu Windows) lub jako serwer Linux na podstawie OpenLogic CentOS 6.6 galerii obrazu (w celu ochrony komputerów Linux).<br/><br/> Opcje zmiany rozmiaru trzy są dostępne — standardowy A4, standardowy D14 i DS4 standardowy.<br/><br/> Serwer jest podłączony do tej samej sieci Azure jako serwer konfiguracji.<br/><br/> Konfigurowanie w Odzyskiwanie witryny portalu | Odbieranych i zachowuje dane replikowane z komputery chronione za pomocą wirtualnych dysków twardych dołączony utworzony na magazyn obiektów blob na koncie Azure miejsca do magazynowania.<br/><br/> Wybierz pozycję Standardowy DS4 specjalnie dla Konfigurowanie ochrony dla obciążenia wymagające spójne wysokiej wydajności i małego opóźnienia przy użyciu konta miejsca do magazynowania Premium.
**Proces serwera** | Lokalnej wirtualną fizycznie serwera lub systemem Windows Server 2012 R2<br/><br/> Zaleca się znajduje się na tej samej sieci lub części sieci LAN jako maszyn, które mają być chronione, ale można uruchamiać w innej sieci, ile maszyn chronionego mają widoczność sieci L3 do niego.<br/><br/> Został on skonfigurowany i zarejestrować ją na serwerze konfiguracji w portalu Odzyskiwanie witryny. | Chroniony maszyn Wyślij dane replikacji do lokalnego serwera proces. Ma dane replikacji pamięci podręcznej pamięci podręcznej na dysku, które otrzymuje. Liczba akcji wykonuje na tych danych.<br/><br/> Optymalizuje danych pamięci podręcznej, kompresji i szyfrowania go przed wysłaniem go na serwerze docelowym wzorca.<br/><br/> Obsługuje ona wypychanych instalacji usługi mobilności.<br/><br/> Wykonuje automatycznego wykrywania maszyn wirtualnych VMware.
**Urządzenia lokalne** | VMware w lokalnym środowisku maszyn wirtualnych systemu lub fizycznie serwery z systemem Windows i Linux oraz. | Można konfigurować ustawienia replikacji co najmniej jednym komputerze. Użytkownik może się nie powieść przez poszczególne komputera lub częściej, wielu komputerach zbiera do planu odzyskiwania. 
**Usługa dla firm — mobilność** | Zainstalowany na poszczególnych maszyn wirtualnych lub serwera fizycznego, które mają być chronione<br/><br/> Można zainstalować ręcznie lub przypisany i instalowane automatycznie przez serwer proces po włączeniu replikacji dla komputera. | Usługa mobilności wysyła danych na serwerze proces podczas replikacji początkowej (ponownej synchronizacji). Po komputerze znajduje się w obecnym stanie (po zakończeniu ponownej synchronizacji) usługa mobilności rejestruje zapisuje na dysku w pamięci i wysyła je na serwerze proces. Za pomocą VSS., uzyskuje się Applicationconsistency dla serwerów systemu Windows
**Azure magazynu Odzyskiwanie witryny** | Tworzenie magazynu Odzyskiwanie witryny z subskrypcją usługi Azure i rejestrowanie serwerów w magazyn. | Magazyn współrzędne i orchestrates replikacji danych, pracy awaryjnej i odzyskiwanie między lokalnej witryny i Azure.
**Mechanizm replikacji** | **Przez Internet**— komunikowanie i replikacja danych z chronionego lokalnego serwerów Azure za pomocą zabezpieczeń SSL/TLS kanał przez internet. To jest domyślna opcja.<br/><br/> **VPN/ExpressRoute**— komunikowanie i replikacja danych między serwerami lokalnego i Azure przez połączenie VPN. Musisz skonfigurować połączenia ExpressRoute między lokalnej witryny i sieci Azure lub połączenie VPN witryny do witryny.<br/><br/> Możesz wybrać sposób mają być replikowane podczas wdrażania Odzyskiwanie witryny. Nie można zmienić mechanizmu po skonfigurowaniu go bez wpływania replikacji maszyn istniejące. | Żadna z tych opcji wymaga otwórz dowolne porty sieci przychodzącego na komputerach chroniony. Całą komunikację sieciową została zainicjowana z lokalnej witryny. 

## <a name="capacity-planning"></a>Planowanie pojemności

Głównych obszarów, które będą potrzebne do rozważenia:

- **Środowiska źródłowego**— VMware infrastruktury, ustawienia komputera źródeł i wymagania.
- **Serwery składników**— proces serwera, serwer konfiguracji i serwer docelowy wzorca 

### <a name="considerations-for-the-source-environment"></a>Zagadnienia dotyczące środowiska źródłowego

- **Maksymalny rozmiar dysku**— bieżący maksymalny rozmiar dysku, który może zostać dołączony maszyn wirtualnych wynosi 1 TB. Dlatego maksymalny rozmiar dysk źródłowy, które mogą być replikowane również jest ograniczone do 1 TB.
- **Maksymalny rozmiar na źródła**— maksymalny rozmiar maszyny pojedynczego źródła jest 31 TB (w przypadku dysków 31) i z wystąpieniem D14 obsługi administracyjnej dla serwera docelowego wzorca. 
- **Liczba źródeł na serwer wzorca docelowej**— wiele komputerów źródła mogą być chronione za pomocą serwera jednego wzorca docelowej. Jednak komputera pojedynczego źródła nie chroniony na wielu serwerach docelowej wzorca, ponieważ jako dyski powielić, wirtualny dysk twardy, która odzwierciedla rozmiar dysku jest tworzona w magazynie obiektów blob platformy Azure i dołączone jako dysk danych na serwerze docelowej wzorca.  
- **Maksymalna codziennie zmiana procentowej dla pojedynczego źródła**— istnieją trzy czynniki, które należy rozważyć Zastanawiając szybkością zalecane Zmień źródło. Aby uzyskać informacje o podstawie docelowej dwóch operacji i/o na SEKUNDĘ są wymagane dysk docelowy dla każdej operacji w źródle. Jest to spowodowane odczytu starych danych i wpisywanie nowych danych wydarzy na dysku docelowym. 
    - **Dzienny zmienić częstotliwość obsługiwana przez serwer proces**— komputerem źródłowym nie może obejmować wielu serwerów proces. Serwer pojedynczy proces może obsługiwać do 1 TB dziennego kursu zmiany. W związku z tym 1 TB jest maksymalna dzienna danych zmienić częstotliwość obsługiwane dla komputera źródła. 
    - **Maksymalna przepustowość obsługiwana przez dysk docelowy**— maksimum pochodząca na dysku źródłowego nie może być więcej niż 144 GB/dzień (o rozmiarze zapisu 8 KB). Zobacz tabelę w sekcji docelowej wzorca przepustowość i operacji i/o na sekundę docelowej dla różnych pisanie rozmiarów. Liczba ta musi być podzielona przez dwa, ponieważ każdego źródła operacji generuje 2 operacji i/o na SEKUNDĘ na dysku docelowym. Przeczytaj o [Azure wydajność i skalowalność elementów docelowych](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) podczas konfigurowania docelowej w przypadku kont miejsca do magazynowania premium.
    - **Maksymalna przepustowość obsługiwane przy użyciu konta z miejsca do magazynowania**— źródła nie może obejmować wielu kont miejsca do magazynowania. Uwagi czy konto miejsca do magazynowania zajmuje maksymalnie 20 000 żądania na sekundę oraz że każdego źródła operacji generuje 2 operacji i/o na SEKUNDĘ na serwerze docelowym wzorca, zalecamy Zachowaj liczbę operacji i/o na SEKUNDĘ w źródle do 10 000. Przeczytaj o [Azure wydajność i skalowalność elementów docelowych](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) podczas konfigurowania źródła w przypadku kont miejsca do magazynowania premium.

### <a name="considerations-for-component-servers"></a>Zagadnienia dotyczące serwerach składników

Tabela 1 zawiera podsumowanie rozmiarów maszyn wirtualnych serwerów docelowych wzorca i konfiguracji.

**Składnik** | **Wdrożonym wystąpienia Azure** | **Rdzenie** | **Pamięci** | **Maksymalna liczba dysków** | **Rozmiar dysku**
--- | --- | --- | --- | --- | ---
Serwer konfiguracji | Standardowy A3 | 4 | 7 GB | 8 | 1023 GB
Serwer docelowy wzorca | Standardowy A4 | 8 | 14 GB | 16 | 1023 GB
 | D14 standardowy | 16 | 112 GB | 32 | 1023 GB
 | Standardowe DS4 | 8 | 28 GB | 16 | 1023 GB

**Tabela 1**

#### <a name="process-server-considerations"></a>Zagadnienia dotyczące procesu serwera

Zazwyczaj zmiany rozmiaru serwera proces zależy od dziennego kursu zmian przez wszystkich chronionych obciążenia.


- Potrzebne do uruchamiania wystarczających do wykonywania zadań, takich jak kompresji w tekście i szyfrowania.
- Serwer proces używa podręcznej przechowywanej na dysku. Upewnij się, że przepustowość miejsca oraz dysku pamięci podręcznej zalecane jest dostępna ułatwiające zmian danych przechowywanych w przypadku gardła sieci lub awarii. 
- Zapewnienia przepustowości wystarczającej, tak, aby serwer procesów można przekazać dane do serwera docelowego wzorca w celu zapewnienia ochrony danych ciągły. 

Tabela 2 zawiera podsumowanie wytyczne dotyczące serwera proces.

**Szybkość zmiany danych** | **PROCESOR** | **Pamięci** | **Rozmiar pamięci podręcznej dysku**| **Przepustowość dysków pamięci podręcznej** | **Przepustowość ingress i wyjściowego**
--- | --- | --- | --- | --- | ---
< 300 GB | 4 vCPUs (2 sockets * 2 rdzenie @ 2,5 GHz) | 4 GB | 600 GB | 7-10 MB na sekundę | 30 MB/s-21 MB/s
300-600 GB | 8 vCPUs (2 sockets * 4 rdzenie @ 2,5 GHz) | 6 GB | 600 GB | 11-15 MB na sekundę | 60 MB/s-42 MB/s
600 GB do 1 TB | 12 vCPUs (2 sockets * 6 rdzenie @ 2,5 GHz) | 8 GB | 600 GB | 16-20 MB na sekundę | 100 MB/s-70 MB/s
> 1 TB | Wdrażanie serwera inny proces | | | | 

**Tabela 2**

W przypadku gdy: 

- Ingress to przepustowość plik do pobrania (intranetowych między serwerem źródła i proces).
- Wyjściowe to przepustowość przekazywania (internet między komputerami serwerów proces i serwera głównego docelowego). Numery wyjściowego uznają średnia kompresja serwera proces 30%.
- Pamięci podręcznej dysku innym dysku OS minimalne 128 GB jest zalecane dla wszystkich serwerów proces.
- Pamięci podręcznej dysku przepustowości użyto następujących magazynowania dla wzorców: 8 dysków skojarzenia zabezpieczeń z 10 OBR z konfiguracją RAID 10.

#### <a name="configuration-server-considerations"></a>Zagadnienia dotyczące konfiguracji serwera

Każdy serwer konfiguracji mogą obsługiwać do 100 maszyn źródła o wielkości 3-4. Jeśli wdrożenia jest większy zalecamy zapoznanie wdrażanie inny serwer konfiguracji. Zobacz Tabela 1 dla maszyn wirtualnych domyślne właściwości serwera konfiguracji. 

#### <a name="master-target-server-and-storage-account-considerations"></a>Zagadnienia konta serwera i magazynowania docelowej wzorca

Magazynowania dla każdego serwera wzorca docelowej zawiera dysk systemu operacyjnego, głośność przechowywania i dyski danych. Dysk przechowywania przechowuje dziennik zmian dysku na czas trwania okna przechowywania zdefiniowane w portalu Odzyskiwanie witryny.  Zapoznaj się z tabeli 1 dla właściwości maszyn wirtualnych serwera docelowego wzorca. Tabela 3 pokazuje, jak korzystać z dysków A4.

**Wystąpienia** | **Dysk systemu operacyjnego** | **Przechowywanie** | **Dyski danych**
--- | --- | --- | ---
 | | **Przechowywanie** | **Dyski danych**
Standardowy A4 | 1 dysku (1 * 1023 GB) | 1 dysku (1 * 1023 GB) | 15 dysków (15 * 1023 GB)
D14 standardowy |  1 dysku (1 * 1023 GB) | 1 dysku (1 * 1023 GB) | 31 dysków (15 * 1023 GB)
Standardowe DS4 |  1 dysku (1 * 1023 GB) | 1 dysku (1 * 1023 GB) | 15 dysków (15 * 1023 GB)

**Tabela 3**

Planowanie dla serwera docelowego wzorca pojemności zależy od:

- Wydajność Azure miejsca do magazynowania i ograniczenia
    - Maksymalna liczba wysoce używać dysków do standardowego maszyny warstwy, jest około 40 (20 000-500 operacji i/o na SEKUNDĘ na dysku) z jednego miejsca do magazynowania konta. Przeczytaj o [cele skalowalność sccounts standardowego magazynu](../storage/storage-scalability-targets.md#scalability-targets-for-standard-storage-accounts) i [premium sccounts miejsca do magazynowania](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts).
-   Dzienny Zmienianie kursu 
-   Miejsce do magazynowania głośność przechowywania.

Należy zauważyć, że:

- Jedno źródło nie może obejmować wielu kont miejsca do magazynowania. Dotyczy to dysk danych przejdź do konta miejsca do magazynowania zaznaczone po skonfigurowaniu ochrony. System operacyjny i dyski przechowywania zazwyczaj przejdź do konta automatycznie wdrożonym miejsca do magazynowania.
- Wielkość miejsca do magazynowania przechowywania wymagane zależy od dziennego kursu zmiany i liczbę dni przechowywania. Miejsca przechowywania, wymaganego na serwer docelowy wzorca = całkowita pochodząca z źródła dziennie * liczba dni przechowywania. 
- Każdy z serwerów docelowych wzorca zawiera tylko jedną objętość przechowywania. Wielkość przechowywania są udostępniane na dyskach dołączone do serwera docelowego wzorca. Na przykład:
    - Jeśli ma komputera źródła, w przypadku dysków 5 i każdego dysku generuje 120 operacji i/o na SEKUNDĘ (rozmiar 8K) w źródle, przekłada się 240 operacji i/o na SEKUNDĘ na dysku (2 operacji na dysku docelowym na źródła ei). Operacji i/o na SEKUNDĘ 240 znajduje się w obrębie Azure na limit operacji i/o na SEKUNDĘ dysku 500.
    - Na wielkość przechowywania staje się on 120 * 5 = 600 operacji i/o na SEKUNDĘ i to może stać się szyjkę butelki. W tym scenariuszu dobrej strategii będzie dodawać nowych dysków do woluminu przechowywania i zakresu go, jako konfiguracji pasek RAID. Spowoduje to zwiększyć wydajność, ponieważ operacji i/o na SEKUNDĘ są rozmieszczane na wiele dysków. Liczba dysków mają zostać dodane do wielkość przechowywania będzie w następujący sposób:
        - Całkowita liczba operacji i/o na SEKUNDĘ ze środowiska źródłowego / 500
        - Suma pochodząca dziennie ze środowiska źródłowego (nieskompresowane) / 287 GB. 287 GB jest maksymalna przepustowość obsługiwana przez dysk docelowy dziennie. Tej metryki różni się w zależności od rozmiaru zapisu przy użyciu współczynnika równego 8K, ponieważ w tym przypadku 8K jest wstążką zakłada, że rozmiar zapisu. Na przykład jeśli rozmiar zapisu jest 4K przepustowość będzie 287/2. I rozmiar zapisu jest 16K przepustowość zostaną 287 * 2.
- Liczba kont miejsca do magazynowania wymagane = Suma źródłowych operacji i/o na sekundę/10 000.


## <a name="before-you-start"></a>Przed rozpoczęciem

**Składnik** | **Wymagania dotyczące** | **Szczegóły**
--- | --- | --- 
**Konto Azure** | Musisz mieć konto [Microsoft Azure](https://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
**Azure miejsca do magazynowania** | Musisz mieć konto Azure przestrzeni dyskowej, aby zapisać zreplikowanej dane<br/><br/> Albo konta powinna być [Konta standardowego zbędne Geo miejsca do magazynowania](../storage/storage-redundancy.md#geo-redundant-storage) lub [Nominalnej miejsca do magazynowania](../storage/storage-premium-storage.md).<br/><br/> Należy w tym samym co usługa Azure Odzyskiwanie witryny, a zostaną skojarzone z tej samej subskrypcji. Przenoszenie kont miejsca do magazynowania utworzone za pomocą [nowego portal Azure](../storage/storage-create-storage-account.md) między grupami zasobów nie jest obsługiwana.<br/><br/> Aby dowiedzieć się więcej, przeczytaj [Wprowadzenie do magazyn Microsoft Azure](../storage/storage-introduction.md)
**Azure wirtualnej sieci** | Konieczne będzie Azure wirtualnej sieci, na którym serwer konfiguracji i wzorca docelowym serwerze zostanie wdrożony. Jako magazynu Odzyskiwanie witryny Azure należy w tej samej subskrypcji i regionu. Jeśli chcesz odtworzyć danych przez połączenie VPN lub ExpressRoute Azure wirtualną sieć musi być połączony z siecią lokalnego przez połączenie ExpressRoute ani VPN witryny do witryny.
**Zasoby Azure** | Upewnij się, że masz za mało Azure zasobów do wdrożenia wszystkie składniki. Przeczytaj więcej o [Limity subskrypcji Azure](../azure-subscription-service-limits.md).
**Azure maszyn wirtualnych** | Maszyn wirtualnych, które mają być chronione powinny być zgodne z [Azure wymagania wstępne](site-recovery-best-practices.md).<br/><br/> **Liczba dysków**— można obsługuje maksymalnie 31 dyskach na serwerze chronionym<br/><br/> **Rozmiary dysku**— poszczególnych pojemność dysku nie powinny być więcej niż 1023 GB<br/><br/> **Klastrowanie**— nie są obsługiwane przez serwery grupowany<br/><br/> **Uruchamiania**— Unified interfejsu sprzętowego Extensible (UEFI) / Extensible Interface(EFI) sprzętowego uruchamiania nie jest obsługiwane<br/><br/> **Ilości**— Bitlocker zaszyfrowanych ilości nie są obsługiwane<br/><br/> **Nazwy serwerów**, nazwy powinny zawierać od 1 do 63 znaków (liter, cyfr i łączników). Nazwa musi rozpoczynać się literą lub cyfrą i kończą się literą lub cyfrą. Po włączeniu ochrony komputerze można zmodyfikować Azure nazwę.
**Serwer konfiguracji** | Standardowy maszyn wirtualnych A3 według Galeria Azure witryny odzyskiwania systemu Windows Server 2012 R2 zostanie utworzona w ramach subskrypcji dla serwera konfiguracji. Zostanie utworzony jako pierwsze wystąpienie w nowej usługi w chmurze. Jeśli wybrano opcję publicznego Internetu, ponieważ typ łączności serwera konfiguracji usługi w chmurze, zostanie utworzony z zastrzeżone publiczny adres IP.<br/><br/> Ścieżka instalacji powinny być tylko litery angielskie.
**Serwer docelowy wzorca** | Azure maszyny wirtualnej, standardowy A4, D14 lub DS4.<br/><br/> Ścieżka instalacji powinny być tylko litery angielskie. Na przykład ścieżkę należy **/usr/local/ASR** dla serwera docelowego wzorca systemem Linux.
**Proces serwera** | Możesz wdrożyć serwer procesów na fizycznej lub maszyn wirtualnych z systemem Windows Server 2012 R2 z najnowszymi aktualizacjami. Instalowanie na C: /.<br/><br/> Zalecamy zapoznanie przełączenie serwera w tej samej sieci i podsieci jako maszyn, którą chcesz chronić.<br/><br/> Zainstaluj VMware vSphere polecenie 5.5.0 na serwerze proces. W celu wykrywania maszyn wirtualnych zarządzane przez serwer vCenter lub maszyn wirtualnych uruchamiania na hoście ESXi składnika VMware vSphere polecenie jest wymagane na serwerze proces.<br/><br/> Ścieżka instalacji powinny być tylko litery angielskie.<br/><br/> System plików odwołań nie jest obsługiwane.
**VMware** | Serwer vCenter VMware zarządzania programu monitorami vSphere VMware. Wersja vCenter 5.1 lub 5.5 powinny być uruchomione z najnowszymi aktualizacjami.<br/><br/> Co najmniej jeden monitorami vSphere zawierające maszyn wirtualnych VMware, który chcesz chronić. Funkcja hypervisor powinna być uruchomiona ESX/ESXi wersji 5.1 lub 5.5 z najnowszymi aktualizacjami.<br/><br/> Maszyn wirtualnych VMware powinien mieć narzędzia VMware zainstalować i uruchomić. 
**Komputery systemu Windows** | Serwery chronione fizycznie lub VMware maszyn wirtualnych systemu Windows ma wiele wymagania.<br/><br/> A obsługiwane 64-bitowy system operacyjny: **Windows Server 2012 R2**, **Windows Server 2012**lub **Windows Server 2008 R2 z w co najmniej z dodatkiem SP1**.<br/><br/> Nazwa hosta, punktów instalacji, nazwy urządzenia, ścieżka systemu Windows (przykład: C:\Windows) powinny być tylko w języku angielskim.<br/><br/> System operacyjny powinien być zainstalowany na dysku C:\.<br/><br/> Tylko dyski podstawowe są obsługiwane. Dyski dynamiczne nie są obsługiwane.<br/><br/> Reguły zapory na komputerach chronionego pozwolić im na serwerach docelowej konfiguracji i wzorca w Azure.p ><p>Konieczne będzie podanie konta administratora (musisz być administratorem lokalnym na komputerze z systemem Windows) do zainstalowania wypychanych usługi mobilności na serwerach systemu Windows. Jeśli podane konto jest kontem domeny nie musisz wyłączyć formant zdalnego dostępu użytkowników na komputerze lokalnym. Aby zrobić to dodanie wpisu rejestru LocalAccountTokenFilterPolicy DWORD o wartości 1 w obszarze HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Aby dodać wpis rejestru z polecenie Otwórz cmd lub programu powershell i wpisz **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. [Aby uzyskać więcej informacji](https://msdn.microsoft.com/library/aa826699.aspx) na temat kontroli dostępu.<br/><br/> Po przełączeniu Jeśli chcesz nawiązać połączenie maszyn wirtualnych systemu Windows w Azure za pomocą pulpitu zdalnego upewnij się, że pulpit zdalny jest włączony na komputerze lokalnym. Jeśli nie łączysz przez sieć VPN, reguły zapory należy zezwolić na połączenia pulpitu zdalnego przez internet.
**Maszyny Linux** | Obsługiwane 64-bitowej wersji systemu operacyjnego: **Centos 6.4, 6.5, 6.6**; **Oracle Enterprise Linux 6.4, 6.5 uruchomionymi jądra zgodne funkcję czerwony lub nierozerwalny Enterprise jądra wersji 3 (UEK3)**, **SUSE Linux Enterprise Server 11 z dodatkiem SP3**.<br/><br/> Reguły zapory na komputerach chronionego pozwolić im na konfiguracji i serwerów docelowych wzorca w Azure.<br/><br/> plików Hosts na komputerach chronionego może zawierać wpisów, które mapowanie nazwy lokalnego hosta na adresy IP skojarzone z wszystkich nic <br/><br/> Jeśli chcesz nawiązać połączenie Azure maszyn wirtualnych systemem Linux po przełączeniu przy użyciu powłoki bezpiecznego klienta (ssh), upewnij się, że usługa bezpiecznego powłoki na komputerze chronionym jest ustawiona na automatycznie uruchamiającego uruchamiania systemu i reguły zapory zezwalać ssh połączenia.<br/><br/> Nazwa hosta, punktów instalacji, nazwy urządzeń i ścieżek systemu Linux i nazwy plików (np-itp-; usr) powinny być tylko w języku angielskim.<br/><br/> Ochrona można włączyć dla komputerów lokalnych z następujących magazynem:-<br>System plików: EXT3, ETX4, ReiserFS, XFS<br>Wielościeżkowe oprogramowanie urządzenia mapowania (wielościeżkowego)<br>Menedżer głośności: LVM2<br>Serwerów fizycznych z nośnikami Kontroler HP CCISS nie są obsługiwane.
**Innych firm** | Niektóre składniki wdrażania, w tym scenariuszu zależą od innej firmy do prawidłowego działania. Aby uzyskać pełną listę zobacz [informacje dotyczące innych firm oprogramowania i informacje](#third-party)


### <a name="network-connectivity"></a>Łączność sieciowa

Istnieją dwie opcje, konfigurując łączność sieciowa między witryny lokalnej i Azure wirtualną sieć wdrażania elementów infrastruktury (serwer konfiguracji, serwerów docelowych wzorca). Musisz zdecydować, sieci łączności opcji przed rozpoczęciem wdrażania serwera konfiguracji. Musisz wybrać to ustawienie w czasie wdrażania. Nie można zmienić później.

**Internet:** Komunikacji i replikacji danych między lokalne serwery (serwer procesów, chronionych maszyn) i serwerach składników Azure infrastruktury (serwer konfiguracji, serwera głównego docelowego) odbywa się przy użyciu bezpiecznego połączenia SSL/TLS ze źródeł lokalnych do publicznych punktów końcowych na serwerach docelowej konfiguracji i wzorzec. (Jedynym wyjątkiem jest połączenie między serwerem proces a serwerem wzorca docelowej na port TCP 9080, czyli niezaszyfrowanym. Tylko sterowania związane z protokołem replikacji konfiguracji replikacji wymieniane informacje dla tego połączenia.)

![Diagram wdrożenia internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: komunikacji i replikacji danych między lokalne serwery (serwer procesów, chronionych maszyn) i serwerach składników Azure infrastruktury (serwer konfiguracji, serwera głównego docelowego) się dzieje przez połączenie VPN między sieci lokalnej i Azure wirtualnych sieci, na którym są rozmieszczane konfiguracji serwera i serwerów docelowych wzorca. Upewnij się, że sieci lokalnej jest połączony Azure wirtualnej sieci przez połączenie ExpressRoute lub połączenie VPN witryny do witryny.

![Diagram wdrożenia sieci VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)


## <a name="step-1-create-a-vault"></a>Krok 1: Tworzenie magazynu

1. Zaloguj się do [portalu zarządzania](https://portal.azure.com).


2. Rozwiń węzeł **Usługi danych** > **Usługi odzyskiwania** i kliknij pozycję **Witryny odzyskiwania magazynu**.


3. Kliknij pozycję **Utwórz nowe** > **Szybkie tworzenie**.

4. W polu **Nazwa**wpisz przyjazną nazwę identyfikującą magazyn.

5. W **regionie**zaznacz regionu geograficznego dla magazyn. Aby sprawdzić obsługiwanych regionów, zobacz dostępność geograficzne w [Azure Szczegóły ceny odzyskiwania witryny](https://azure.microsoft.com/pricing/details/site-recovery/)

6. Kliknij przycisk **Utwórz magazynu**.

    ![Nowe magazynu](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Sprawdź na pasku stanu, aby potwierdzić, że magazyn pomyślnie został utworzony. Magazyn zostaną wyświetlone na stronie głównej **Usługi odzyskiwania** jako **aktywną** .

## <a name="step-2-deploy-a-configuration-server"></a>Krok 2: Wdrażanie serwera konfiguracji

### <a name="configure-server-settings"></a>Konfigurowanie ustawień serwera

1. Na stronie **Usługi odzyskiwania** kliknij pozycję Magazyn, aby otworzyć stronę Szybki Start. Szybki Start można również otworzyć w dowolnym momencie za pomocą ikony.

    ![Ikona Szybki Start](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)

2. Na liście rozwijanej wybierz **między lokalnej witryny z serwerami VMware/fizycznej i Azure**.
3. **Przygotowywanie Target(Azure)** zasobów kliknij pozycję **Wdrożyć serwer konfiguracji**.

    ![Wdrażanie serwera konfiguracji](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)

4. W **Nowej konfiguracji serwera szczegóły** określ:

    - Nazwa konfiguracji serwera i poświadczenia, aby połączyć się z nim.
    - W polu Typ połączenia sieciowego listy rozwijanej wybierz pozycję **Publicznego Internetu** lub **sieci VPN**. Należy zauważyć, że nie można zmodyfikować to ustawienie, po jego zastosowaniu.
    - Wybierz sieć Azure, na którym powinien znajdować się serwer. Jeśli korzystasz z sieci VPN upewnij się, że Azure sieć jest podłączona do sieci lokalnej zgodnie z oczekiwaniami. 
    - Określ wewnętrzny adres IP i podsieci, który zostanie przypisany do serwera. Należy zauważyć, że pierwsze cztery adresów IP w dowolnym podsieci są zarezerwowane dla wewnętrznych zastosowania Azure. Za pomocą innych dostępnych adresów IP.
    
    ![Wdrażanie serwera konfiguracji](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)

5. Po kliknięciu przycisku **OK** standardowy maszyny wirtualnej A3 według Galeria Azure witryny odzyskiwania systemu Windows Server 2012 R2 zostanie utworzona w ramach subskrypcji dla serwera konfiguracji. Zostanie utworzony jako pierwsze wystąpienie w nowej usługi w chmurze. W przypadku wybrania łączą przez internet usługa w chmurze, zostanie utworzona i zastrzeżone publiczny adres IP. Można monitorować postęp na karcie **zadania** .

    ![Monitorowanie postępu](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)

6.  Jeśli łączysz się przez internet, po serwer konfiguracji wdrożonym notatki publiczny adres IP przydzielono go na stronie **maszyn wirtualnych** w Azure portal. Następnie na **punkty końcowe** Uwaga kartę publicznej port HTTPS mapowane na prywatne na porcie 443. Musisz te informacje później, podczas rejestrowania docelowej wzorca i serwery proces z serwera konfiguracji. Serwer konfiguracji jest wdrażane za te punkty końcowe:

    - HTTPS: Publicznej portu jest używane do koordynowania komunikacji między serwerami składnik i Azure przez internet. Prywatne na porcie 443 są używane do koordynowania komunikacji między serwerami składnik i Azure przez sieć VPN.
    - Niestandardowe: Publicznej port jest używany dla komunikacji narzędzie awarii w Internecie. Port prywatny 9443 służy do awarii narzędzia komunikacji w sieci VPN.
    - Programu PowerShell: Port prywatny 5986
    - Pulpit zdalny: prywatne portu 3389
    
    ![Punkty końcowe maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

    >[AZURE.WARNING] Nie usuwać i zmieniać numer portu publiczny czy prywatny punkty końcowe, wszelkie utworzone podczas wdrażania serwera konfiguracji.

Serwer konfiguracji zostanie wdrożony w usłudze tworzonych automatycznie chmura Azure z zastrzeżonego adresu IP. Zastrzeżony adres jest potrzebny do upewnij się, że adres IP usługi cloud konfiguracji serwera pozostaje taki sam przez ponownego uruchamiania maszyn wirtualnych (łącznie z serwera konfiguracji) w usłudze w chmurze. Zastrzeżone publiczny adres IP trzeba ręcznie niezarezerwowana serwer konfiguracji jest zlikwidować lub będzie nadal zastrzeżone. Istnieje domyślny limit 20 zastrzeżone publiczne adresy IP, na subskrypcję. [Aby uzyskać więcej informacji](../virtual-network/virtual-networks-reserved-private-ip.md) na temat zastrzeżone adresy IP. 

### <a name="register-the-configuration-server-in-the-vault"></a>Rejestrowanie serwera konfiguracji w Magazyn

1. Na stronie **Szybkie uruchamianie** kliknij pozycję **Zasoby przygotowywanie docelowej** > **pobrać klucza rejestru**. Plik klucza jest generowany automatycznie. Jest ważne przez 5 dni po jest generowany. Skopiuj go do serwera konfiguracji.
2. W **środowisku maszyn wirtualnych** wybierz serwer konfiguracji z listy maszyn wirtualnych. Otwórz kartę **pulpitu nawigacyjnego** i kliknij przycisk **Połącz**. **Otwórz** pobrany plik RDP zalogować się do serwera konfiguracji pulpitu zdalnego. Jeśli korzystasz z sieci VPN, za pomocą połączenia pulpitu zdalnego z lokalnej witryny wewnętrzny adres IP (adresu określonym przez Ciebie podczas wdrożyć serwer konfiguracji). Kreatora konfiguracji serwera konfiguracji Azure witryny odzyskiwania jest uruchamiany automatycznie po zalogowaniu się po raz pierwszy.

    ![Rejestracja](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)

3. Podczas **Instalacji oprogramowania innych firm** kliknij pozycję **akceptuję** pobrać i zainstalować MySQL.

    ![Zainstaluj MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)

4. **Szczegóły serwer MySQL** można utworzyć poświadczenia, aby zalogować się na wystąpienie serwera MySQL.

    ![Poświadczenia MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)

5. W obszarze **Ustawienia internetowej** Określ, jak serwer konfiguracji połączy się z Internetem. Należy zauważyć, że:

    - Jeśli chcesz używać niestandardowego serwera proxy został on skonfigurowany przed rozpoczęciem instalacji dostawcy.
    - Po kliknięciu przycisku **Dalej** , aby sprawdzić połączenie serwera proxy spowoduje uruchomienie test.
    - Używanie niestandardowej serwera proxy, czy serwer proxy domyślne wymaga uwierzytelniania, musisz wprowadzić szczegółów serwera proxy, takich jak adres, port i poświadczenia.
    - Następujące adresy URL powinna być dostępna za pośrednictwem serwera proxy:
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Jeśli masz oparte na adresie IP reguły zapory upewnij się, że zasady są ustawione zezwalają na komunikację z serwera konfiguracji adresów IP opisanego w [Zakresów adresów IP centrum danych Azure](https://msdn.microsoft.com/library/azure/dn175718.aspx) i HTTPS protocol (443). Będą musiały biała lista zakresów adresów IP Azure regionu, który ma zostać użycia, po czym US Zachód.

    ![Rejestracja serwera proxy](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)

6. W **Ustawieniach lokalizacji komunikat o błędzie dostawcy** określ w języka, w którym są wyświetlane komunikaty o błędach.

    ![Rejestracja komunikat o błędzie](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)

7. W **Rejestracji odzyskiwania witryny Azure** Przeglądaj i wybierz plik klucza, który został skopiowany na serwerze.

    ![Plik klucza rejestru](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)

8. Po zakończeniu stronie kreatora zaznacz następujące opcje:

    - Wybierz pozycję **Uruchamianie zarządzania okno dialogowe konto** , aby określić, że okno dialogowe Zarządzaj kontami ma zostać otwarty po zakończeniu pracy z kreatorem.
    - Wybierz opcję **Utwórz ikony na pulpicie służącej do Cspsconfigtool** , aby dodać skrót na serwer konfiguracji, tak aby w dowolnym momencie można otworzyć okno dialogowe **Zarządzaj kontami** , bez konieczności ponownie uruchomić kreatora.

    ![Zakończenie rejestracji](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)

9. Kliknij przycisk **Zakończ** , aby zakończyć działanie kreatora. Hasło jest generowany. Skopiuj go w bezpiecznej lokalizacji. Musisz go do uwierzytelniania i zarejestrować proces i serwerów docelowych wzorca z serwera konfiguracji. Służy także do zapewnienia integralności kanału w komunikacji z serwerami konfiguracji. Można odtworzyć hasło, ale następnie musisz ponownie zarejestrować docelowej wzorca i procesu serwerów przy użyciu nowego hasła.

    ![Hasło](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Po zarejestrowaniu serwer konfiguracji zostaną wyświetlone na stronie **Konfiguracja serwerów** magazyn.

### <a name="set-up-and-manage-accounts"></a>Konfigurowanie kont i zarządzania nimi

Podczas rozmieszczania Odzyskiwanie witryny żąda poświadczeń dla następujących czynności:

- Konto VMware dzięki thatSite odzyskiwania można automatycznie maszyny wirtualne odnajdowania na serwerach vCenter lub vSphere hosts. 
- Po dodaniu maszyny do ochrony, tak, aby Odzyskiwanie witryny można zainstalować usługę mobilności nad nimi.

Po została zarejestrowana serwer konfiguracji można otworzyć okno dialogowe **Zarządzaj kontami** , aby dodać i zarządzać kontami, które powinny być używane do tych akcji. Istnieje kilka sposobów wykonaj następujące czynności:

- Otwórz skrót, który użytkownik wybrał utworzone dla okna dialogowego na ostatniej stronie konfiguracji serwera konfiguracji (cspsconfigtool).
- Otwieranie okna dialogowego na zakończenie konfiguracji serwera konfiguracji.

1. W **Zarządzaj kontami** kliknij pozycję **Dodaj konto**. Można także modyfikowanie i usuwanie istniejącego konta.

    ![Zarządzanie kontami](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)

2. W **Szczegółowe informacje o koncie** Określ nazwę konta, aby użyć w Azure i poświadczeń (nazwy użytkownika i domeny). 

    ![Zarządzanie kontami](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Nawiązywanie połączenia z serwerem konfiguracji 

Istnieją dwa sposoby nawiązywania połączenia z serwerem konfiguracji:

- Przez sieć VPN witryny do witryny ani ExpressRoute
- Przez internet 

Należy zauważyć, że:

- Połączenie z Internetem używa punkty końcowe maszyny wirtualnej w połączeniu z wirtualną publicznego adresu IP serwera.
- Połączenie VPN używa wewnętrzny adres IP serwera razem z porty prywatne punktu końcowego.
- Jest jednorazowa decyzja zdecydować, czy nawiązania połączenia (dane kontroli i replikacji) z serwerów lokalnego różnych serwerach składników (serwer konfiguracji, serwera głównego docelowego) działa w Azure przez połączenie VPN lub w Internecie. Nie można zmienić to ustawienie później. Jeśli musisz ponownie wdróż tego scenariusza i reprotect komputery.  


## <a name="step-3-deploy-the-master-target-server"></a>Krok 3: Wdrażanie serwera docelowego wzorca

1. Kliknij przycisk **Przygotuj zasoby Target(Azure)** > **Wdrażanie serwera docelowego wzorca**.
2. Określ serwer szczegółowe informacje o docelowej wzorca i poświadczenia. Serwer zostanie wdrożony w tej samej sieci Azure jako serwer konfiguracji. Po kliknięciu do wykonania Azure maszyn wirtualnych zostanie utworzony przy użyciu systemu Windows i Linux oraz obraz galerii.

    ![Ustawienia serwera docelowej](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Należy zauważyć, że pierwsze cztery adresów IP w dowolnym podsieci są zarezerwowane dla wewnętrznych zastosowania Azure. Określ inne dostępne adres IP.

>[AZURE.NOTE] Wybierz pozycję Standardowy DS4, podczas konfigurowania ochrony dla obciążenia wymagających spójne wysoka wydajność wejścia/wyjścia i małego opóźnienia w celu obsługi we/wy zadań intensywnie przy użyciu [Konta miejsca do magazynowania Premium](../storage/storage-premium-storage.md).


3. Windows obszary serwera docelowego, utworzone maszyn wirtualnych z tych punktów końcowych. Należy zauważyć, że publicznej punkty końcowe są tworzone tylko w przypadku nawiązywania połączenia przez internet.

    - Niestandardowe: Publicznej port używany przez serwer proces wysyłanie replikacji danych przez internet. Prywatne port 9443 jest używany przez serwer proces przesyłanie danych replikacji na serwerze docelowej wzorca w sieci VPN.
    - Niestandardowa 1: Port publicznej używanego przez serwer proces, aby wysłać metadanych w Internecie. Prywatne port 9080 jest używany przez serwer proces przesyłanie metadanych do wzorca docelowego serwera w sieci VPN.
    - Programu PowerShell: Port prywatny 5986
    - Pulpit zdalny: prywatne portu 3389

4. Serwer docelowy wzorca Linux maszyn wirtualnych zostanie utworzona i te punkty końcowe. Należy zauważyć, że publicznej punkty końcowe są tworzone tylko wtedy, gdy nawiązujesz połączenie przez internet.

    - Niestandardowe: Publicznej port używany przez serwer proces wysyłanie replikacji danych przez internet. Prywatne port 9443 jest używany przez serwer proces przesyłanie danych replikacji na serwerze docelowej wzorca w sieci VPN.
    - Niestandardowa 1: Publicznej port jest używany przez serwer proces wysłać metadanych w Internecie. Prywatne port 9080 jest używany przez serwer procesów przesyłanie metadanych do wzorca docelowego serwera w sieci VPN
    - SSH: Port prywatny 22

    >[AZURE.WARNING] Nie usuwać i zmieniać numer portu publiczny czy prywatny dowolnej punkty końcowe utworzone podczas wdrażania serwera docelowej wzorca.

5. W **środowisku maszyn wirtualnych systemu** Czekaj na maszyny wirtualnej rozpocząć.

    - Jeśli jest notatkę systemu Windows server w dół dane pulpitu zdalnego.
    - Jeśli jest on serwer Linux i nawiązujesz połączenie VPN notatce wewnętrzny adres IP maszyny wirtualnej. Jeśli nawiązujesz połączenie przez internet Uwaga publiczny adres IP.

6.  Zaloguj się do serwera do ukończenia instalacji i zarejestrować ją z serwera konfiguracji. 
7.  Jeśli używasz systemu Windows:

    1. Rozpocznij połączenie pulpitu zdalnego maszyn wirtualnych. Przy pierwszym zalogowaniu skrypt będzie działać w oknie programu PowerShell. Nie, zamknij go. Po zakończeniu narzędzia konfiguracji agenta hosta zostanie otwarty automatycznie Aby zarejestrować serwer.
    2. W **Konfiguracji agenta hosta** Określ wewnętrzny adres IP serwera konfiguracji i 443. Nawet jeśli nie łączysz przez sieć VPN ponieważ maszyny wirtualnej jest dołączona do tej samej sieci Azure jako serwer konfiguracji można użyć adresu wewnętrznego i prywatnych na porcie 443. Pozostaw **HTTPS Użyj** włączone. Wprowadź hasło serwera konfiguracji, który wspomniano wcześniej. Kliknij **przycisk OK** , aby zarejestrować serwer. Możesz zignorować opcje translatora adresów Sieciowych. Nie są używane.
    3. Jeśli z wymaganiami dysk przechowywania szacowany jest więcej niż 1 TB można skonfigurować wielkość przechowywania (R:) za pomocą dysku wirtualnego i [spacje miejsca do magazynowania](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx)
    
    ![System Windows server docelowej wzorca](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)

8. Jeśli używasz Linux:
    1. Upewnij się, że zainstalowano najnowszą Linux oraz integracja usług (LIS) przed instalacją serwera docelowego wzorca. Możesz znaleźć najnowszą wersję pakietu LIS wraz z instrukcjami na temat instalacji [tutaj](https://www.microsoft.com/download/details.aspx?id=46842). Po zainstalowaniu LIS, uruchom ponownie komputer.
    2. W **zasoby przygotowywanie docelowej (Azure)** kliknij przycisk **Pobierz i zainstaluj dodatkowego oprogramowania (tylko dla serwera docelowej wzorzec Linux)**. Skopiuj plik pobrany tar maszyn wirtualnych za pomocą klienta sftp. Można zalogować się do serwera docelowego wzorcowej wdrożonym linux i używać *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* do pobierania pliku.
    2. Zaloguj się na serwerze przy użyciu powłoki bezpiecznego klienta. Jeśli masz połączenie z siecią Azure przez sieć VPN za pomocą wewnętrzny adres IP. W przeciwnym razie użyj zewnętrzny adres IP i punkt końcowy publicznej SSH.
    3. Wyodrębnij pliki z Instalatorem gzipped, uruchamiając: **tar — xvzf firmy Microsoft — ASR_UA_8.4.0.0_RHEL6 — 64***
    ![Linux wzorcowej docelowym serwerze](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
    4. Upewnij się, że jest się w katalogu, do którego zostały wyodrębnione zawartość pliku tar.
    5. Skopiuj hasło serwera konfiguracji do lokalnego pliku za pomocą polecenia * *echo* `<passphrase>` * > passphrase.txt**
    6. Uruchom polecenie "**sudo. Zainstaluj -t obu - hosta -R -d /usr/local/ASR MasterTarget -i* `<Configuration server internal IP address>` * -p 443 -s t - c https -P passphrase.txt**".

    ![Zarejestruj się w docelowym serwerze](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)

9. Poczekaj kilka minut (10-15) i na stronie Sprawdź wymieniony serwer docelowy wzorca, zarejestrowanej w **Serwery** > kartę **Szczegóły serwera** **Serwerami konfiguracji** . Jeśli korzystasz z systemu Linux, a nie zarejestrował hoście narzędzie konfiguracji ponownie uruchom z /usr/local/ASR/Vx/bin/hostconfigcli. Należy ustawić uprawnienia dostępu, uruchamiając chmod jako główny.

    ![Sprawdź serwer docelowy](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

>[AZURE.NOTE] Może potrwać do 15 minut po zakończeniu rejestracji dla serwera docelowego wzorca będzie widoczny w portalu. Aby zaktualizować od razu, kliknij przycisk **Odśwież** na stronie **Konfiguracja serwerów** .

## <a name="step-4-deploy-the-on-premises-process-server"></a>Krok 4: Wdrażanie lokalnego serwera proces

Przed rozpoczęciem zaleca się skonfigurować statyczny adres IP na serwerze proces tak, aby ma gwarancję ma być trwała wśród ponownego uruchamiania.

1. Kliknij pozycję Szybki Start > **lokalnego serwera proces instalacji** > **Pobieranie i instalowanie serwera proces**.

    ![Instalowanie serwera proces](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)

2.  Skopiuj plik zip pobrany na serwerze, na którym zamierzasz zainstalować serwer proces. Plik zip zawiera dwa pliki instalacji:

    - Microsoft — ASR_CX_TP_8.4.0.0_Windows*
    - Microsoft — ASR_CX_8.4.0.0_Windows*

3. Rozpakuj plik archiwum i skopiuj pliki instalacji w lokalizacji na serwerze.
4. Uruchamianie **firmy Microsoft — ASR_CX_TP_8.4.0.0_Windows*** instalacji plik i postępuj zgodnie z instrukcjami. Spowoduje to zainstalowanie składników innych firm potrzebnych do wdrożenia.
5. Następnie uruchom **firmy Microsoft — ASR_CX_8.4.0.0_Windows***.
6. Na stronie **Trybu serwera** wybierz **Serwer proces**.
7. Na stronie **Szczegóły środowiska** wykonaj następujące czynności:


    - Jeśli chcesz chronić VMware virtual urządzenia kliknij przycisk **Tak**
    - Jeśli chcesz tylko Ochrona serwerów fizycznych i dlatego nie ma potrzeby vCLI VMware zainstalowany na serwerze proces. Kliknij przycisk **nie** i Kontynuuj.

8. Weź pod uwagę następujące podczas instalowania VMware vCLI:

    - **Jest obsługiwana tylko VMware vSphere polecenie 5.5.0**. Proces serwera nie działa z innymi wersjami i aktualizacjami vSphere interfejsu wiersza polecenia.
    - Pobierz vSphere polecenie 5.5.0 z [tutaj.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
    - Jeśli zainstalowano vSphere polecenie tuż przed rozpoczęciem instalacji serwera proces i Instalator nie wykryje go, poczekaj pięć minut, zanim zostanie ponownie spróbuj przeprowadzić instalację. Dzięki temu, że wszystkie zmienne środowiska potrzebne do wykrywania polecenie vSphere zostały zainicjowane poprawnie.

9.  W **Wyborze NIC dla serwera proces** Wybierz sieciową, który ma być używany przez serwer procesów.

    ![Wybierz kartę](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)

10. W **konfiguracji serwera szczegóły**:

    - Adres IP i port Jeśli chcesz się łączyć przez sieć VPN Określ wewnętrzny adres IP serwera konfiguracji i 443 portu. Określanie w inny sposób publicznej wirtualny adres IP i zamapowanych publicznej końcowego HTTP.
    - Wpisz hasło serwera konfiguracji.
    - Wyczyść **Sprawdź mobilności usługi oprogramowania podpisu** , jeśli chcesz wyłączyć weryfikacji, gdy umożliwia automatyczne wypychanych zainstalować usługę. Weryfikacja podpisu musi łączność z Internetem z serwera proces.
    - Kliknij przycisk **Dalej**.

    ![Zarejestruj się w konfiguracji serwera](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)


11. W **instalacji wybierz dysk** wybierz dysk pamięci podręcznej. Serwer proces wymaga dysku pamięci podręcznej z co najmniej 600 GB wolnego miejsca. Następnie kliknij przycisk **Zainstaluj**. 

    ![Zarejestruj się w konfiguracji serwera](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)

12. Należy zauważyć, że może być konieczne ponowne uruchomienie serwera, aby ukończyć instalację. W polu **Serwer konfiguracji** > **Szczegóły serwera** Sprawdź, czy serwer proces zostanie wyświetlony został pomyślnie zarejestrowany w magazyn.

>[AZURE.NOTE]Może potrwać do 15 minut po zakończeniu rejestracji na serwerze proces wyświetlane jako wymienione w obszarze Serwer konfiguracji. Aby zaktualizować od razu, Odśwież serwer konfiguracji, klikając przycisk Odśwież w dolnej części strony konfiguracji serwera
 
![Sprawdzanie serwera proces](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Jeśli nie wyłączyć Weryfikacja podpisu w usłudze mobilności, podczas rejestrowania serwera proces, możesz to zrobić później, w następujący sposób:

1. Zaloguj się do serwera proces jako administrator i Otwórz plik C:\pushinstallsvc\pushinstaller.conf do edycji. W sekcji **[PushInstaller.transport]** Dodaj poniższy wiersz: **SignatureVerificationChecks = "0"**. Zapisz i zamknij plik.
2. Uruchom ponownie usługę InMage PushInstall.


## <a name="step-5-update-site-recovery-components"></a>Krok 5: Składniki Odzyskiwanie witryny aktualizacji

Składniki odzyskiwania witryny zostaną zaktualizowane od czasu do czasu. Gdy będą dostępne nowe aktualizacje, należy zainstalować je w następującej kolejności:

1. Serwer konfiguracji
2. Proces serwera
3. Serwer docelowy wzorca
4. Narzędzie awarii (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Uzyskiwanie i instalowanie aktualizacji


1. Odzyskiwanie witryny **pulpitu nawigacyjnego**można uzyskać aktualizacje konfiguracji, proces i serwerów docelowych wzorca. Do zainstalowania Linux Wyodrębnij pliki z Instalatorem gzipped i uruchom polecenie "sudo. Zainstaluj" Aby zainstalować aktualizację.
2. [Pobierz](http://go.microsoft.com/fwlink/?LinkID=533813) najnowszą aktualizację dla tool(vContinuum) awarii.
3. Jeśli pracujesz w środowisku maszyn wirtualnych systemu lub serwerów fizycznych, które już zainstalowana usługa mobilności, można uzyskać aktualizacji usługi w następujący sposób:

    - **Opcja 1**: pobieranie aktualizacji:
        - [Windows Server (64-bitowe tylko)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
        - [6.4,6.5,6.6 centOS (64-bitowe tylko)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
        - [Oracle Enterprise Linux 6.4,6.5 (64-bitowe tylko)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
        - [SUSE Linux Enterprise Server z dodatkiem SP3 (64-bitowe tylko)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
        - Po zaktualizowaniu serwera proces zaktualizowanej wersji usługi mobilności będą dostępne w folderze C:\pushinstallsvc\repository na serwerze proces.
    - **Opcja 2**: Jeśli masz komputer za pomocą starszej wersji zainstalowana usługa mobilności, można automatycznie uaktualnić usługi mobilności na komputerze za pomocą portalu zarządzania.

        1. Upewnij się, serwer proces aktualizacji.
        2. Upewnij się, że komputer chroniony spełnia [wymagania wstępne](#install-the-mobility-service-automatically) dotyczące automatycznie naciśnięcie usługę mobilności, tak, aby aktualizacji działa zgodnie z oczekiwaniami.
        2. Wybierz grupę ochrony, wyróżnij chronionego komputera, a następnie kliknij przycisk **mobilności aktualizacji usługi**. Ten przycisk jest dostępna tylko jeśli istnieje nowsza wersja usługi mobilności. 

            ![Wybierz serwer vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

W wybierz pozycję konta określ konta administratora, który ma być używany do usługi mobilności na serwerze chronionym aktualizacji. Kliknij przycisk OK i poczekaj, aż wyzwalane zadania do wykonania.


## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Krok 6: Dodaj serwery vCenter lub vSphere hostów

1. Kliknij pozycję **Serwery** > **Serwerami konfiguracji** > konfiguracji serwera >**Dodaj vCenter serwera** , aby dodać hosta serwera lub vSphere vCenter.

    ![Wybierz serwer vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)

2. Określ szczegóły dotyczące serwera lub hosta i wybierz serwer proces, używanym do znalezienia go.

    - Jeśli serwer vCenter nie jest uruchomiony na porcie 443 domyślnym Określ numer portu, na którym jest uruchomiony na serwerze vCenter.
    - Serwer proces musi znajdować się na tej samej sieci hosta serwera/vSphere vCenter i powinien mieć VMware vSphere polecenie 5.5.0 zainstalowany.

    ![ustawienia serwera vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)


3. Po zakończeniu odnajdowania serwera vCenter zostaną wyświetlone w obszarze Szczegóły konfiguracji serwera.

    ![ustawienia serwera vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)

4. Jeśli korzystasz z konta innego niż administrator Dodawanie serwera lub hosta, upewnij się, że konto ma następujące uprawnienia:

    - konta vCenter powinny mieć centrum danych, magazynu danych, folderu, hosta, sieci, zasobów, miejsca do magazynowania widoków, maszyn wirtualnych i vSphere Distributed Przełącz uprawnienia.
    - konta hosta vSphere powinny mieć centrum danych, magazynu danych, folderu, hosta, sieci, zasobów, maszyn wirtualnych i vSphere Distributed Przełącz uprawnienia



## <a name="step-7-create-a-protection-group"></a>Krok 7: Tworzenie grupy ochrony

1. Otwieranie **Elementów chroniony** > **Grupa ochrona** > **Tworzenie ochrony grupy**.

    ![Tworzenie grupy ochrony](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)

2. Na stronie **Określanie ustawień grupy ochrony** Określ nazwę grupy i wybierz serwer konfiguracji, na którym chcesz utworzyć grupę.

    ![Ustawienia grupy ochrony](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)

3. Na stronie **Określanie ustawień replikacji** Konfigurowanie ustawień replikacji, które będą używane dla wszystkich komputerów w grupie.

    ![Replikacja grupy ochrony](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)

4. Ustawienia:
    - **Wiele maszyn wirtualnych spójności**: jeśli ją włączyć tworzy punkty udostępnionego spójną z aplikacją odzyskiwania na komputerach w grupie ochrona. To ustawienie jest najbardziej odpowiednie podczas wszystkich urządzeń w grupie ochrona z tym samym obciążenie pracą. Wszystkie komputery zostaną przywrócone do tego samego punktu danych. Przycisk dostępny tylko dla serwerów systemu Windows.
    - **Próg RPO**: alerty zostaną wygenerowane podczas replikacji ochrony danych ciągłego RPO przekracza skonfigurowaną wartość progowa RPO.
    - **Odzyskiwanie punktu przechowywania**: Określa okna przechowywania. Chroniony maszyn może zostać przywrócona do dowolnego punktu w tym oknie.
    - **Częstotliwość migawkę spójną z aplikacją**: Określa, jak często będą tworzone punkty odzyskiwania zawierające migawek spójnych aplikacji.

Grupa ochrona można monitorować, jak są generowane na stronie **Chronionych elementów** .

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Krok 8: Konfigurowanie komputerów, które mają być chronione

Musisz zainstalować usługę mobilności w środowisku maszyn wirtualnych systemu i serwerów fizycznych, którą chcesz chronić. Można to zrobić na dwa sposoby:

- Automatycznie push i zainstalować usługę na każdym komputerze z serwera proces.
- Ręcznie zainstalować usługę. 

### <a name="install-the-mobility-service-automatically"></a>Instalowanie usługi mobilności automatycznie

Po dodaniu do grupy ochrony komputerów usługa mobilności automatycznie przypisany i zainstalowany na każdym komputerze przez serwer proces. 

**Automatycznie wypychanych zainstalować usługę mobilności na serwerach systemu Windows:** 

1. Instalowanie najnowszych aktualizacji dla serwera proces, zgodnie z opisem w [krok 5: Instalowanie najnowszych aktualizacji](#step-5-install-latest-updates)i upewnij się, że serwer proces jest dostępny. 
2. Upewnij się, istnieje połączenie sieciowe między komputerem źródłowym a serwerem proces i że komputerem źródłowym jest dostępny na serwerze proces.  
3. Konfigurowanie Zapory systemu Windows umożliwia **Udostępnianie plików i drukarek** i **Oprzyrządowania zarządzania systemu Windows**. W obszarze Ustawienia Zapory systemu Windows wybierz opcję "Zezwolić aplikacji lub funkcji na dostęp przez zaporę" i wybierz programy, jak pokazano na poniższym obrazie. W przypadku komputerów, które należą do domeny można skonfigurować zasad zapory z obiektem zasad grupy.

    ![Ustawienia zapory](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)

4. Konto używane do wykonywania instalacji wypychanych należy do grupy Administratorzy na komputerze, który chcesz chronić. Te poświadczenia są używane tylko wypychanych instalacji usługi mobilności i podasz ich podczas dodawania komputera do grupy ochrony.
5. Jeśli podane konto nie jest kontem domeny musisz wyłączyć formant zdalnego dostępu użytkowników na komputerze lokalnym. Aby zrobić to dodanie wpisu rejestru LocalAccountTokenFilterPolicy DWORD o wartości 1 w obszarze HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Aby dodać wpis rejestru z polecenie Otwórz cmd lub programu powershell i wpisz **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. 

**Automatycznie wypychanych zainstalować usługę mobilności na serwerach Linux:**

1. Instalowanie najnowszych aktualizacji dla serwera proces, zgodnie z opisem w [krok 5: Instalowanie najnowszych aktualizacji](#step-5-install-latest-updates)i upewnij się, że serwer proces jest dostępny.
2. Upewnij się, istnieje połączenie sieciowe między komputerem źródłowym a serwerem proces i że komputerem źródłowym jest dostępny na serwerze proces.  
3. Upewnij się, że konto jest użytkownika root na serwerze źródłowym Linux.
4. Należy się upewnić, że plik Hosts na serwerze Linux źródła zawiera wpisów, które mapowanie nazwy lokalnego hosta na adresy IP skojarzone z wszystkich nic.
5. Zainstaluj najnowszą openssh, openssh serwera, openssl pakietów na komputerze, który chcesz chronić.
6. Upewnij się, że SSH jest włączona i uruchomiona na porcie 22. 
7. Włącz uwierzytelnianie podsystemu i hasło SFTP w pliku sshd_config w następujący sposób: 

    - ) Zaloguj się jako główny.
    - b) w /etc/ssh/sshd_config pliku Znajdź wiersz, który zaczyna się od **PasswordAuthentication**.
    - c) Usuń komentarze wiersza i zmień wartość "nie", "tak".

        ![Linux oraz dla firm — mobilność](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)

    - d) Znajdź wiersz, który zaczyna się od podsystemu i Usuń komentarze wiersza.
    
        ![Mobilność wypychanych Linux](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    

8. Upewnij się, że wariant Linux komputera źródła jest obsługiwana. 
 
### <a name="install-the-mobility-service-manually"></a>Ręcznie zainstalować usługę mobilności

Pakietów oprogramowania użyte do zainstalowania usługi mobilności znajdują się na serwerze proces w C:\pushinstallsvc\repository. Zaloguj się do serwera proces i skopiuj pakiet instalacyjny odpowiednie do komputera źródła oparta na tabeli poniżej:-

| Źródłowego systemu operacyjnego                               | Mobilność pakietu usługi na serwerze proces                                                            |
|---------------------------------------------------    |------------------------------------------------------------------------------------------------------ |
| Windows Server (64-bitowe tylko)                          | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe`         |
| CentOS 6.4, 6.5, 6.6 (64-bitowe tylko)                    | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz`     |
| SUSE Linux Enterprise Server 11 z dodatkiem SP3 (64-bitowe tylko)     | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz`|
| Oracle Enterprise Linux 6.4, 6.5 (64-bitowe tylko)        | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz`       |


**Aby zainstalować usługę mobilności ręcznie w systemie Windows server**, wykonaj następujące czynności:

1. Skopiuj pakietu **Microsoft ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** z serwera proces ścieżką wymienionych w powyższej tabeli, na tym komputerze źródła.
2. Zainstaluj usługę mobilności, uruchamiając plik wykonywalny na komputerze źródła.
3. Postępuj zgodnie z instrukcjami Instalatora.
4. Wybierz **usługę mobilności** rolę, a następnie kliknij przycisk **Dalej**.
    
    ![Instalowanie usługi dla firm — mobilność](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)

5. Pozostaw katalogu instalacyjnego jako domyślnej ścieżki instalacji, a następnie kliknij przycisk **Zainstaluj**.
6. W **Konfiguracji agenta hosta** Określ adres IP i port HTTPS serwera konfiguracji.

    - Jeśli nawiązujesz połączenie przez internet Określ publicznej wirtualny adres IP i publicznej końcowego HTTPS jako portu.
    - Jeśli nawiązujesz połączenie przez sieć VPN Określ wewnętrzny adres IP i 443 portu. Zaznaczona opcja Opuść **Użyj HTTPS** .

    ![Instalowanie usługi dla firm — mobilność](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)

7. Określ hasło konfiguracji serwera i kliknij **przycisk OK** , aby zarejestrować usługi mobilności z serwera konfiguracji.

**Aby uruchomić z poziomu wiersza polecenia:**

1. Kopiowanie hasło z CX do pliku "C:\connection.passphrase" na serwerze i tego polecenia. W naszym przykładzie CX i 104.40.75.37 oraz portu HTTPS jest 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Instalowanie usługi mobilności ręcznie na serwerze Linux**:

1. Kopiowanie archiwum odpowiednie tar oparta na tabeli powyżej, z serwera proces komputerem źródłowym.
2. Otwórz program powłoki i wyodrębnianie archiwum zip tar na ścieżkę lokalną przez wykonywanie`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Tworzenie pliku passphrase.txt w katalogu lokalnego, do którego zostały wyodrębnione zawartość archiwum tar wprowadzając *`echo <passphrase> >passphrase.txt`* z powłoki.
4. Instalowanie usługi mobilności wprowadzając *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Określ adres IP i port:

    - Jeśli łączysz się z serwerem konfiguracji w Internecie określić konfiguracji serwera wirtualnego publiczny adres IP i publicznej końcowego HTTPS w `<IP address>` i `<port>`.
    - Jeśli łączysz się przez połączenie VPN Określ wewnętrzny adres IP i 443.

**Aby uruchomić z poziomu wiersza polecenia**:

1. Kopiowanie hasło z CX do pliku "passphrase.txt" na serwerze i uruchom tego polecenia. W naszym przykładzie CX i 104.40.75.37 oraz portu HTTPS jest 62519:

Aby zainstalować na serwerze produkcyjnym:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt
 
Aby zainstalować na serwerze docelowym:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

>[AZURE.NOTE] Podczas dodawania komputerów do grupy ochrony uruchomione odpowiednią wersję usługi mobilności następnie instalacji wypychanych jest pomijany.


## <a name="step-9-enable-protection"></a>Krok 9: Włącz ochronę

Aby włączyć ochronę możesz dodać maszyn wirtualnych i serwerów fizycznych do grupy ochrony. Przed rozpoczęciem należy zauważyć, że:

- Maszyn wirtualnych zostaną wykryte co 15 minut, a może potrwać do 15 minut pojawiały się w Odzyskiwanie witryny Azure po odnajdowania.
- Zmiany środowiska na komputerze wirtualnych (na przykład instalacji narzędzia VMware) można także korzystać 15 minut mają być aktualizowane w Odzyskiwanie witryny.
- Możesz sprawdzić podczas ostatniego odkryte w polu **Ostatniego kontaktu na** hosta serwera/ESXi vCenter na stronie **Konfiguracja serwerów** .
- Jeśli masz już utworzony grupy ochrony i Dodawanie hosta serwera lub ESXi vCenter po wykonaniu tej trwa 15 minut dla portal Azure Odzyskiwanie witryny, aby odświeżyć i maszyn wirtualnych będzie widoczny w oknie dialogowym **Dodawanie maszyny do grupy ochrony** .
- Jeśli chcesz od razu kontynuować dodawanie maszyny do ochrony grupy nie czekając na odnajdowanie według harmonogramu, podświetl serwer konfiguracji (nie klikaj jej) i kliknij przycisk **Odśwież** .
- Po dodaniu maszyn wirtualnych lub fizycznych komputerów do grupy ochrony serwera proces automatycznie umieszcza i instaluje usługę mobilności na serwerze źródłowym, jeśli tego nie został już zainstalowany.
- Mechanizm automatycznego wypychanych pracy upewnij się, że masz skonfigurowany chronionego komputery, zgodnie z opisem w poprzednim kroku.

Dodawanie maszyny w następujący sposób:

1. Kliknij pozycję **chronionych elementów** > **Grupa ochrona** > **maszyny** > **Dodawanie maszyny**. Zgodnie z zaleceniami dotyczącymi zalecamy ochrony należy lustrzane usługi Obciążenie pracą, aby dodać komputerów z systemem określonej aplikacji do tej samej grupy.
2. W **środowisku maszyn wirtualnych wybierz** Jeśli masz Ochrona serwerów fizycznych, w kreatorze **Dodawanie fizycznych komputerów** podać adres IP i przyjazną nazwę. Następnie wybierz z rodziny system operacyjny.

    ![Dodawanie serwera Centrum V](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)

3. W **środowisku maszyn wirtualnych wybierz** , jeśli masz ochrona maszyn wirtualnych VMware, wybierz serwer vCenter, która jest używana do zarządzania maszyn wirtualnych (lub hosta EXSi, na którym jest uruchomiona), a następnie wybierz maszyny.

    ![Dodawanie serwera Centrum V](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png) 
4. **Określ** zasobów docelowej wybierz serwerów docelowych wzorca i miejsca do magazynowania dla replikacji oraz określ, czy ustawienia powinny być używane do wszystkich obciążenia. Wybierz [Konto miejsca do magazynowania Premium](../storage/storage-premium-storage.md) podczas konfigurowania ochrony dla obciążenia wymagających spójne wysokiej wydajności Jo i małego opóźnienia w celu obsługi Jo intensywnie zadań. Jeśli chcesz za pomocą konta Premium miejsca do magazynowania dla dysków obciążenie pracą, należy użyć docelowej wzorzec Zasadami serii. Nie można używać dysków w magazynie Premium wzorzec docelowej nie serii DS.

    >[AZURE.NOTE] Przenoszenie kont miejsca do magazynowania utworzone za pomocą [nowego portal Azure](../storage/storage-create-storage-account.md) między grupami zasobów nie jest obsługiwana.

    ![Serwer vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)

5. W **Określ konta** wybierz konto, dla którego chcesz użyć instalowania usługi mobilności na komputerach chroniony. Poświadczenia konta są wymagane przez automatyczną instalację usługi mobilności. Jeśli nie możesz wybrać konto upewnij się, że możesz skonfigurować jedną zgodnie z opisem w kroku 2. Należy zauważyć, że to konto nie są dostępne dla Azure. Dla systemu Windows server konta powinien mieć uprawnienia administratora na serwerze źródłowym. Linux konta musi być głównym.

    ![Poświadczenia Linux](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)

6. Kliknij znacznik wyboru, aby zakończyć dodawanie maszyny do grupy ochrony i uruchomić replikacji początkowej dla każdego komputera. Można monitorować stan na stronie **zadania** .

    ![Dodawanie serwera Centrum V](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)

7. Ponadto można monitorować stanu ochrony przez kliknięcie **Pozycji chroniony** > Nazwa grupy ochrony > **maszyn wirtualnych** . Po zakończeniu replikacji początkowej i maszyny jest wykonywana synchronizacja danych przedstawiają **chronione** stanu.

    ![Zadania maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)


### <a name="set-protected-machine-properties"></a>Ustawianie chroniony maszynowego właściwości

1. Po komputerze znajduje się w stanie **chronione** można skonfigurować jego właściwości pracy awaryjnej. W zakresie ochrony szczegóły grupy wybierz komputer i otwórz kartę **Konfiguruj** .
2. Możesz zmienić nazwę, która będzie przypisywany do komputera w Azure po awaryjnego i rozmiar Azure maszyn wirtualnych. Można też zaznaczyć Azure sieci, na którym komputerze będzie połączony po przełączeniu.

    > [AZURE.NOTE] [Migracja sieci](../resource-group-move-resources.md) wzdłuż grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane dla sieci używane do wdrażania Odzyskiwanie witryny.

    ![Ustawianie właściwości maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Należy zauważyć, że:

- Nazwa komputera, Azure musi spełniać wymagania Azure.
- Domyślnie zreplikowanej maszyn wirtualnych w Azure nie masz połączenia z siecią Azure. Chcąc zreplikowanej maszyn wirtualnych można komunikować się upewnij się ustawić tej samej sieci Azure dla nich.
- Jeśli rozmiar woluminu na serwerze fizycznej lub wirtualnej maszyny VMware przechodzi do stanu krytyczne. Jeśli potrzebujesz zmienić rozmiar, wykonaj następujące czynności:

    - ) Zmień ustawienie rozmiaru.
    - b) na karcie **maszyn wirtualnych** zaznacz maszyny wirtualnej, a następnie kliknij przycisk **Usuń**.
    - c) w **Usuwanie maszyn wirtualnych** wybierz opcję **wyłączyć ochronę przed (do użycia w przypadku odzyskiwania przechodzić do i głośność, zmienianie rozmiaru)**. Ta opcja powoduje wyłączenie ochrony, ale zachowuje punktów odzyskiwania w Azure.

        ![Ustawianie właściwości maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)

    - d) ponownie włączyć ochronę maszyny wirtualnej. Po ponownym włączeniu ochrony danych po zmianie rozmiaru woluminu zostanie przeniesiona do Azure.

    

## <a name="step-10-run-a-failover"></a>Krok 10: Uruchom w trybie awaryjnym

Obecnie można uruchamiać tylko niezaplanowane praca awaryjna dla chronionego maszyn wirtualnych VMware i serwerów fizycznych. Pamiętaj o następujących kwestiach:



- Zanim rozpoczniesz trybie awaryjnym zapewnić serwerów docelowych konfiguracji i wzorzec działa i jest prawidłowy. W przeciwnym razie pracy awaryjnej nie powiedzie się.
- Maszyny źródła zamknięty nie są częścią nieplanowanego przełączania awaryjnego. Wykonywanie nieplanowanego przełączania awaryjnego przestaje replikacji danych dla serwerów chroniony. Musisz usunąć maszyny z grupy ochrony i dodać je ponownie, aby uruchomić ponownie ochrony komputerów po zakończeniu nieplanowanego przełączania awaryjnego.
- Jeśli chcesz zakończyć się niepowodzeniem nad bez utraty danych, upewnij się, że maszyn wirtualnych podstawowej witryny są wyłączone, zanim rozpoczniesz tym przełączeniu.

1. Na **Plany odzyskiwania** strony i dodać plan odzyskiwania. Określ szczegóły planu i wybierz **Azure** jako miejsce docelowe.

    ![Konfigurowanie planu odzyskiwania](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)

2. W **Zaznacz maszyn wirtualnych** wybierz grupę ochrona, a następnie wybierz komputery w grupie, aby dodać do planu odzyskiwania. [Dowiedz się więcej](site-recovery-create-recovery-plans.md) o planach odzyskiwania.

    ![Dodawanie maszyn wirtualnych](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)

3. Jeśli potrzebne można dostosować plan tworzenie grup i sekwencji kolejności maszyn w odzyskiwania plan są nie powiodła się nad. Można również dodać monit o podanie ręcznego akcji i skryptów. Odzyskiwanie Azure skryptów można dodawać przy użyciu [Runbooks automatyzacji Azure](site-recovery-runbook-automation.md).

4. Na stronie **Plany odzyskiwania** wybierz plan, a następnie kliknij przycisk **Nieplanowanego przełączania awaryjnego**.
5. W **Pracy awaryjnej upewnij się,** Sprawdź kierunek pracy awaryjnej (aby Azure) i wybierz punkt odzyskiwania przechodzić do.
6. Poczekaj, aż pracy awaryjnej zadanie do wykonania, a następnie sprawdź, czy tym przełączeniu powiodło się zgodnie z oczekiwaniami i że zreplikowanej maszyn wirtualnych pomyślne uruchomienie Azure.




## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Krok 11: Niepowodzenie ponownie nie powiodło się na komputerach z platformy Azure

[Aby uzyskać więcej informacji](site-recovery-failback-azure-to-vmware-classic-legacy.md) na temat dotyczące usługi nie powiodło się nad komputerów z systemem Azure z powrotem do środowiska lokalnego.


## <a name="manage-your-process-servers"></a>Zarządzanie serwery proces

Serwer procesów wysyła danych replikacji na serwerze wzorca docelowej platformy Azure, a zostanie odnaleziona nowych maszyn wirtualnych VMware dodane do serwera vCenter. W poniższych przypadkach może chcesz zmienić serwer procesów we wdrożeniu programu:

- Jeśli bieżący serwer proces został wyłączony
- Jeśli do odzyskiwania punktów cel (RPO) wzrośnie do poziomu do przyjęcia dla Twojej organizacji.

W razie potrzeby możesz przenieść replikacji niektórych lub wszystkich VMware w lokalnym środowisku maszyn wirtualnych systemu i serwerów fizycznych na serwerze inny proces. Na przykład:

- **Błąd**— Jeśli serwer proces kończy się niepowodzeniem, lub nie jest dostępna możesz przenieść replikacji chronionego komputera na inny serwer proces. Metadane komputerem źródłowym i maszynowego replice zostaną przeniesione do nowego serwera proces i dane są zsynchronizowane. Nowy serwer proces automatycznie połączy się z serwerem vCenter umożliwia automatyczne odnajdowanie. Można monitorować stan procesu serwery na pulpicie nawigacyjnym Odzyskiwanie witryny.
- **Aby dostosować RPO do równoważenia**— równoważenia możesz ulepszona obciążenia wybierz serwer inny proces w portalu Odzyskiwanie witryny, a przenieść replikacji co najmniej jednym komputerze do niego do równoważenia obciążenia ręcznego. W tym przypadku metadanych zaznaczonego urządzeń źródła i replice jest przenoszona do nowego serwera proces. Oryginalny serwer proces pozostaje nawiązanie połączenia z serwerem vCenter. 

### <a name="monitor-the-process-server"></a>Monitorowanie serwera proces

Jeśli serwer proces jest w stanie krytyczne ostrzeżenie o stanie będą wyświetlane na pulpicie nawigacyjnym witryny odzyskiwania. Kliknięcie stanu, aby otworzyć kartę zdarzenia, a następnie Drąż w dół do określonych zadań na karcie zadania. 

### <a name="modify-the-process-server-used-for-replication"></a>Modyfikowanie serwera proces replikacji

1. Otwórz **Serwery** > **Serwerami konfiguracji** > konfiguracji serwera > **Szczegóły serwera**.
2. Kliknij pozycję **Serwery proces** > **Zmień proces serwer** obok serwera, który chcesz zmodyfikować.

    ![Zmienianie serwera proces 1](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)

3. **Zmienianie proces**Server > **Serwera proces docelowego** wybierz nowy serwer chcesz użyć, a następnie wybierz pozycję maszyn wirtualnych, które mają być replikowane na nowy serwer. Kliknij ikonę informacji obok nazwy serwera Szczegóły wolnego miejsca i używanych pamięci. Średnia miejsce, do którego będą musieli się replikowane każdego wybrana maszyna wirtualna na serwerze nowy proces zostanie wyświetlona ułatwiające ładowanie decyzji.

    ![Zmienianie serwera proces 2](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)

4. Kliknij znacznik wyboru, aby rozpocząć replikacji do nowego serwera proces. Należy zauważyć, że po usunięciu wszystkich maszyn wirtualnych na serwerze procesu, który był krytyczne go powinna już wyświetlane krytyczne ostrzeżenie na pulpicie nawigacyjnym.


## <a name="third-party-software-notices-and-information"></a>Uwagi innych firm oprogramowania i informacje

Nie tłumaczenie i Localize

Oprogramowania i układowego produktu firmy Microsoft lub usługa jest oparty na lub zawiera materiał z projekty wymienione poniżej (zbiorowo, "Kod innych firm").  Firma Microsoft jest nie twórca kodu innych firm.  Oryginalny o prawach autorskich i licencji, pod którym firma Microsoft otrzymała takiego kodu innych firm są ustawione przedstawione poniżej.

Informacje znajdujące się w sekcji A dotyczy składniki kodu innych firm z projekty wymienione poniżej. Te licencje i informacje są udostępniane tylko w celach informacyjnych.  Kod innej firmy jest podejmowana relicensed użytkownikowi przez firmę Microsoft w obszarze postanowień licencyjnych oprogramowania firmy Microsoft dotyczące produktów firmy Microsoft lub usługi.  

Informacje podane w sekcji B dotyczy składników kod innej firmy, które są udostępniane użytkownikowi przez firmę Microsoft w obszarze oryginalny postanowienia licencyjne.

Ukończono plików można znaleźć w witrynie [Centrum pobierania Microsoft](http://go.microsoft.com/fwlink/?LinkId=529428). Firma Microsoft zastrzega sobie wszystkie prawa nie udzielone w niniejszej Umowie, czy za tym idzie, estoppel lub w inny sposób.
