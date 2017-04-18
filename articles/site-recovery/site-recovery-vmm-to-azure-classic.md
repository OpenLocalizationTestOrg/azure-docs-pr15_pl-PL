<properties
    pageTitle="Powielić maszyn wirtualnych funkcji Hyper-V w chmury VMM Azure | Microsoft Azure"
    description="Ten artykuł zawiera opis sposobu replikacji maszyn wirtualnych funkcji Hyper-V na znajdującej się w chmury VMM Centrum systemu Azure hosts funkcji Hyper-V."
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
    ms.topic="hero-article"
    ms.date="05/06/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Replikacja maszyn wirtualnych funkcji Hyper-V w chmury VMM Azure

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmm-to-azure.md)
- [PowerShell — Menedżera zasobów](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasyczny portalu](site-recovery-vmm-to-azure-classic.md)
- [PowerShell — klasyczny](site-recovery-deploy-with-powershell.md)



Usługa Azure witryny odzyskiwania składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Urządzenia mogą być replikowane Azure lub centrum danych lokalnych pomocniczą. Krótki przegląd odczytywanie [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md).

## <a name="overview"></a>Omówienie

Ten artykuł zawiera opis sposobu wdrażania Odzyskiwanie witryny replikacji maszyn wirtualnych funkcji Hyper-V na serwerach hosta funkcji Hyper-V znajdujących się w VMM chmury prywatne Azure.

W tym artykule zawiera wymagania wstępne dotyczące tego scenariusza i pokazano, jak skonfigurować magazynu Odzyskiwanie witryny, uzyskiwanie dostawcy odzyskiwania witryny Azure zainstalowanego na serwerze VMM źródłowym, zarejestrować serwer w magazyn, Dodaj konto Azure miejsca do magazynowania, zainstalować agenta usługi Azure odzyskiwania na serwerach hosta funkcji Hyper-V i konfigurowanie ustawień ochrony dla chmur VMM, które będą stosowane do wszystkich chronionych maszyn wirtualnych , a następnie włączyć ochronę tych maszyn wirtualnych. Zakończyć, testując przełączanie awaryjne do upewnij się, że wszystko działa zgodnie z oczekiwaniami.

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.

## <a name="architecture"></a>Architektura

![Architektura](./media/site-recovery-vmm-to-azure-classic/topology.png)

- Dostawca odzyskiwania Azure witryny jest zainstalowana na VMM podczas wdrażania Odzyskiwanie witryny i serwer VMM jest rejestrowany w magazynu Odzyskiwanie witryny. Dostawca komunikuje się z Odzyskiwanie witryny do obsługi aranżacji replikacji.
- Agent usługi Azure odzyskiwania jest zainstalowany na serwerach hosta funkcji Hyper-V podczas wdrażania Odzyskiwanie witryny. Obsługuje ona replikacji danych do magazynu Azure.


## <a name="azure-prerequisites"></a>Wymagania wstępne dotyczące Azure

Oto, czego potrzebujesz platformy Azure.

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Konto Azure**| Musisz mieć konto [Microsoft Azure](https://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.
**Azure miejsca do magazynowania** | Musisz mieć konto Azure miejsca do magazynowania, aby zapisać zreplikowanej dane. Replikowane dane są przechowywane w magazynie Azure i maszyny wirtualne Azure są surowa w górę, gdy wystąpi awaryjne przeniesienie. <br/><br/>Wymagane jest [konto standardowego magazynu geo zbędne](../storage/storage-redundancy.md#geo-redundant-storage). Konta muszą w tym samym co usługa Odzyskiwanie witryny, a zostaną skojarzone z tej samej subskrypcji. Należy zauważyć, że replikacja kont miejsca do magazynowania premium nie jest obecnie obsługiwane i nie powinny być używane.<br/><br/>[Przeczytaj o](../storage/storage-introduction.md) Miejsce do magazynowania Azure.
**Sieć Azure** | Konieczne będzie Azure wirtualnej sieci, który maszyny wirtualne Azure będzie łączyć się, gdy wystąpi awaryjne przeniesienie. Azure wirtualną sieć musi być w tym samym regionie jako magazynu Odzyskiwanie witryny.

## <a name="on-premises-prerequisites"></a>Wymagania wstępne dotyczące lokalnego

Oto, czego potrzebujesz lokalnej.

**Wymagania wstępne** | **Szczegóły**
--- | ---
**VMM** | Konieczne będzie co najmniej jeden serwer VMM wdrożony jako serwer autonomiczny fizycznej lub wirtualnej lub jako klaster wirtualny. <br/><br/>Serwer VMM powinien być uruchamiany System Center 2012 R2 z najnowszych aktualizacji skumulowana.<br/><br/>Konieczne będzie co najmniej jedna chmura skonfigurowany na serwerze VMM.<br/><br/>W chmurze źródła, które mają być chronione musi zawierać jedną lub więcej grup hosta VMM.<br/><br/>Dowiedz się więcej o konfigurowaniu chmur VMM w [Instruktaż: tworzenie prywatnych chmury za pomocą systemu Centrum 2012 z dodatkiem SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) w blogu Jan Mayer.
**Funkcji Hyper-V** | Musisz Serwery hosta funkcji Hyper-V lub klastrów w chmurze VMM. Serwer hosta powinien mieć i co najmniej jeden maszyny wirtualne. <br/><br/>Serwer funkcji Hyper-V musi być uruchomiony co najmniej **Windows Server 2012 R2** z roli Hyper-V lub **Microsoft Hyper-V Server 2012 R2** i zainstalowano najnowsze aktualizacje.<br/><br/>Dowolny serwer funkcji Hyper-V zawierający maszyny wirtualne mają być chronione musi znajdować się w chmurze VMM.<br/><br/>Jeśli używasz funkcji Hyper-V w notatce klaster tego brokera klaster nie jest tworzone automatycznie, jeśli masz statyczne klaster oparty na adresie IP. Musisz ręcznie skonfigurować brokera klaster. [Aby uzyskać więcej informacji](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) w Aidan Finn wpis w blogu.
**Chroniony maszyn** | Maszyny wirtualne, które mają być chronione powinny spełniać [wymagania Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).


## <a name="network-mapping-prerequisites"></a>Wymagania wstępne dotyczące sieci mapowania
Przy ochrony maszyn wirtualnych w mapach mapowania Azure sieci między sieciami maszyn wirtualnych na serwerze VMM źródłowym i docelowych Azure sieciach, zapewniając następujące czynności:

- Które pracy awaryjnej w tej samej sieci można połączyć ze sobą, niezależnie od tego, który plan odzyskiwania znajdują się w wszystkich komputerach.
- Jeśli brama sieci jest skonfigurowana w celu Azure sieci, maszyn wirtualnych można nawiązać pozostałych maszyn wirtualnych lokalnego.
- Jeśli nie skonfigurowano sieci mapowania tylko maszyn wirtualnych, które kończą się niepowodzeniem nad w tego samego planu odzyskiwania będzie mógł połączyć się ze sobą po przełączeniu Azure.

Jeśli chcesz wdrożyć mapowanie sieci najpierw następujące czynności:

- Maszyn wirtualnych, które mają być chronione na serwerze źródłowym VMM powinny być połączone z siecią maszyn wirtualnych. Sieci należy połączyć z siecią logiczną, która jest skojarzony z chmury.
- Sieć Azure, do której można podłączyć zreplikowanej maszyn wirtualnych po przełączeniu. Możesz wybrać tej sieci w czasie pracy awaryjnej. W tym samym regionie jako subskrypcji Odzyskiwanie witryny Azure powinny być sieci.

Przygotowywanie się do sieci mapowanie w następujący sposób:

1. [Przeczytaj o](site-recovery-network-mapping.md) wymagań dotyczących mapowania sieci.
2. Przygotowywanie maszyn wirtualnych sieci VMM:

    - [Konfigurowanie sieci logicznych](https://technet.microsoft.com/library/jj721568.aspx).
    - [Konfigurowanie maszyn wirtualnych sieci](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Krok 1: Tworzenie magazynu Odzyskiwanie witryny

1. Zaloguj się do [Portalu zarządzania](https://portal.azure.com) na serwerze VMM, które chcesz zarejestrować.
2. Kliknij pozycję **dane usługi** > **usługi odzyskiwania** > **witryny odzyskiwania magazynu**.
3. Kliknij pozycję **Utwórz nowe** > **Szybkie tworzenie**.
4. W polu **Nazwa**wpisz przyjazną nazwę identyfikującą magazyn.
5. W **regionie**zaznacz regionu geograficznego dla magazyn. Aby sprawdzić obsługiwanych regionów Zobacz dostępność geograficzne w [Azure Szczegóły ceny odzyskiwania witryny](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Kliknij przycisk **Utwórz magazynu**.

    ![Nowe magazynu](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Sprawdź na pasku stanu, aby potwierdzić, że magazyn pomyślnie został utworzony. Magazyn zostaną wyświetlone na stronie głównej usługi odzyskiwania jako **aktywną** .

## <a name="step-2-generate-a-vault-registration-key"></a>Krok 2: Generuj klucz rejestru magazynu

Generowanie klucza rejestru w magazyn. Po pobraniu dostawca odzyskiwania witryny Azure i zainstalować go na serwerze VMM, aby zarejestrować serwer VMM w magazyn za pomocą tego klucza.

1. Na stronie **Usługi odzyskiwania** kliknij pozycję Magazyn, aby otworzyć stronę Szybki Start. Szybki Start można również otworzyć w dowolnym momencie za pomocą ikony.

    ![Ikona Szybki Start](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)

2. Na liście rozwijanej wybierz **między lokalnej witryny VMM i Microsoft Azure**.
3. **Przygotowywanie serwerów VMM**kliknij plik **klucza rejestru Generuj** . Plik klucza jest generowane automatycznie i obowiązuje 5 dni po jest generowany. Jeśli Azure portal nie korzysta z serwera VMM musisz skopiować ten plik na serwerze.

    ![Klucz rejestru](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Krok 3: Instalowanie dostawcy odzyskiwania Azure witryny

1. W **Przewodniku Szybki Start** > **Przygotowywanie VMM serwerów**, kliknij przycisk **Pobierz Microsoft Azure odzyskiwania dostawca witryny do instalacji na serwerach VMM** uzyskać najnowszą wersję pliku instalacyjnego dostawcy.
2. Uruchom ten plik na serwerze VMM źródłowym.

    >[AZURE.NOTE] Jeśli VMM zostanie wdrożony w klastrze i instalujesz dostawcę po raz pierwszy zainstalować go na aktywny węzeł i zakończyć instalację, aby zarejestrować serwer VMM w magazyn. Następnie zainstalować dostawcę na innych węzłach. Uwaga Jeśli uaktualniania dostawcę konieczne będzie uaktualnienie we wszystkich węzłach, ponieważ ta osoba może wszystkie działać w tej samej wersji dostawcy.

3. Instalator sprawdza prerequirements i żąda uprawnień, aby zatrzymać usługę VMM, aby rozpocząć instalację dostawcy. Usługa VMM zostanie automatycznie uruchomiona, po zakończeniu instalacji. Jeśli przeprowadzasz instalację w klastrze VMM zostanie wyświetlony monit, aby zatrzymać roli klaster.

4. W **Witrynie Microsoft Update** można jednak aktualizacje. To ustawienie jest włączone dostawcy aktualizacje zostaną zainstalowane zgodnie z zasadami usługi Microsoft Update.

    ![Aktualizacje firmy Microsoft](./media/site-recovery-vmm-to-azure-classic/updates.png)


5.  Lokalizacja instalacji dla dostawcy jest ustawiona na ** <SystemDrive>\Program Files\Microsoft System Centrum 2012 R2\Virtual komputera Manager\bin**. Kliknij przycisk **Zainstaluj**.

    ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)

6. Po zainstalowaniu dostawcy kliknij przycisk **Zarejestruj** , aby zarejestrować serwer w magazyn.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)

9. W polu **Nazwa magazynu**Sprawdź nazwę magazynu, w którym zostanie zarejestrowany serwera. Kliknij przycisk *Dalej*.

    ![Rejestracja serwera](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)

7. W oknie **Połączenie internetowe** określ jak dostawcy na serwerze VMM łączy się z Internetem. Wybierz pozycję **Połącz z istniejących ustawień serwera proxy** , aby korzystać z domyślnych ustawień połączenia internetowego skonfigurowany na serwerze.

    ![Ustawienia internetowej](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

    - Jeśli chcesz używać niestandardowego serwera proxy został on skonfigurowany przed rozpoczęciem instalacji dostawcy. Podczas konfigurowania ustawień serwera proxy niestandardowy, aby sprawdzić połączenie serwera proxy spowoduje uruchomienie test.
    - Używanie niestandardowej serwera proxy, czy serwer proxy domyślne wymaga uwierzytelniania, musisz wprowadzić szczegóły serwera proxy, w tym adres serwera proxy oraz portu.
    - Po adresy URL powinien być dostępny na serwerze VMM i hostów funkcji Hyper-v
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Zezwalaj opisanych w [Zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) i HTTPS protocol (443) adresów IP. Trzeba biała lista zakresów adresów IP Azure regionu, który ma zostać użycia i US Zachód.
    - Jeśli korzystasz z niestandardowej serwera proxy konta VMM RunAs (DRAProxyAccount) jest tworzona automatycznie przy użyciu poświadczeń określonego serwera proxy. Konfigurowanie serwera proxy, aby to konto można uwierzytelnić pomyślnie. W konsoli VMM można modyfikować ustawienia konta VMM RunAs. W tym celu należy otworzyć obszar roboczy **ustawień** , rozwiń węzeł **Zabezpieczenia**, kliknij polecenie **Uruchom jako konta**, a następnie zmodyfikuj hasła dla DRAProxyAccount. Musisz ponownie uruchomić usługę VMM, tak aby to ustawienie jest stosowane.


8. W polu **Klucz rejestru**wybierz klucz, który pobrane z Odzyskiwanie witryny Azure i skopiować na serwerze VMM.


10.  Ustawienie szyfrowania jest używane tylko w przypadku, gdy masz replikacji maszyny wirtualne funkcji Hyper-V w chmury VMM Azure. Jeśli masz replikacji do pomocniczej witryny nie służy.

11.  W polu **Nazwa serwera**Określ przyjazną nazwę identyfikującą serwer VMM w magazyn. W konfiguracji klaster Określ nazwę VMM klaster roli.
12.  W **metadanych chmury Synchronizuj** wybierz czy chcesz zsynchronizować z magazynu metadanych dla wszystkich chmur na serwerze VMM. Ta akcja musi tylko raz wystąpić na wszystkich serwerach. Jeśli nie chcesz zsynchronizować wszystkie chmury, można pozostawić niezaznaczone to ustawienie i synchronizowanie każdego chmury pojedynczo w chmurze właściwości w konsoli VMM.

13.  Kliknij przycisk **Dalej** , aby ukończyć proces. Po zarejestrowaniu metadanych na serwerze VMM jest pobierana przez Odzyskiwanie witryny Azure. Serwer jest wyświetlany na karcie **Serwery VMM** na stronie **Serwery** magazyn.

    ![LastPage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Po zarejestrowaniu metadanych na serwerze VMM jest pobierana przez Odzyskiwanie witryny Azure. Serwer jest wyświetlany na karcie **Serwery VMM** na stronie **Serwery** magazyn.

### <a name="command-line-installation"></a>Instalacja wiersza polecenia

Dostawca odzyskiwania Azure witryny można również zainstalować przy użyciu następującego polecenia. Ta metoda umożliwia instalowanie dostawcy na serwerze Core dla systemu Windows Server 2012 R2.

1. Pobierz klucz pliku i rejestracji instalacji dostawcy do folderu. Na przykład: C:\ASR.
2. Zatrzymaj usługę System Center Virtual Machine Manager
3. W wierszu polecenia z podwyższonym poziomem uprawnień wyodrębnić Instalatora dostawcy za pomocą tych poleceń:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Instalowanie dostawcy w następujący sposób:

        C:\ASR> setupdr.exe /i

5. Zarejestruj dostawcę w następujący sposób:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Gdzie są parametry w następujący sposób:

 - **/Credentials** : obowiązkowe parametr, który określa lokalizację, w której znajduje się plik klucza rejestru  
 - **/FriendlyName** : parametr obowiązkowe dla nazwy serwera hosta funkcji Hyper-V, który pojawia się w portalu Odzyskiwanie witryny Azure.
 - **/EncryptionEnabled** : parametr opcjonalny, aby określić, czy mają zostać szyfrowania maszyn wirtualnych w Azure (w pozostałych szyfrowanie). Nazwa pliku powinien mieć rozszerzenie **pfx** .
 - **/proxyAddress** : parametr opcjonalny, który określa adres serwera proxy.
 - **/proxyport** : parametr opcjonalny, który określa port serwera proxy.
 - **/proxyUsername** : parametr opcjonalny określający nazwę użytkownika serwera proxy.
 - **/proxyPassword** : parametr opcjonalny, który określa hasło serwera proxy.  


## <a name="step-4-create-an-azure-storage-account"></a>Krok 4: Utwórz konto Azure miejsca do magazynowania

1. Jeśli nie masz konta usługi Azure magazynowania kliknij pozycję **Dodaj konto Azure przestrzeni dyskowej** , aby utworzyć konto.
2. Utwórz konta z replikacją geo, włączony. Należy w tym samym co usługa Azure Odzyskiwanie witryny, a zostaną skojarzone z tej samej subskrypcji.

    ![Konto miejsca do magazynowania](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [AZURE.NOTE] [Migracja kont miejsca do magazynowania](../resource-group-move-resources.md) w grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane w przypadku kont miejsca do magazynowania na potrzeby wdrażania Odzyskiwanie witryny.

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Krok 5: Instalowanie agenta usługi Azure odzyskiwania

Instalowanie agenta usługi Azure odzyskiwania na wszystkich serwerach hosta funkcji Hyper-V w chmurze VMM.

1. Kliknij pozycję **Szybki Start** > **Pobierz agenta usług odzyskiwania witryny Azure i zainstaluj na hosts** uzyskać najnowszą wersję pliku instalacji agenta.

    ![Instalowanie agenta odzyskiwania usług](./media/site-recovery-vmm-to-azure-classic/install-agent.png)

2. Uruchom plik instalacji na wszystkich serwerach hosta funkcji Hyper-V.
3. Na stronie **Sprawdź wymagania wstępne** kliknij przycisk **Dalej**. Wymagania wstępne dotyczące wszelkie brakujące zostanie zainstalowany automatycznie.

    ![Wymagania wstępne dotyczące odzyskiwania usług agenta](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)

4. Na stronie **Ustawienia instalacji** określ miejsce, w którym chcesz zainstalować agenta i wybierz lokalizację pamięci podręcznej, w którym zostanie zainstalowana metadanych kopii zapasowej. Następnie kliknij przycisk **Zainstaluj**.
5. Po zakończeniu instalacji kliknij przycisk **Zamknij** kreatora.

    ![Agent MARS rejestru](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Instalacja wiersza polecenia

Możesz również zainstalować agenta usługi Microsoft Azure odzyskiwania z poziomu wiersza polecenia przy użyciu tego polecenia:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Krok 6: Konfigurowanie chmury ustawień ochrony

Po zarejestrowaniu serwera VMM można konfigurować ustawienia ochrony chmury. Opcja **Synchronizuj dane chmur z magazyn** jest włączone, gdy zainstalowano dostawcę, więc wszystkich chmur na serwerze VMM pojawią się na karcie <b>Chronionych elementów</b> w magazyn.

![Opublikowanych chmury](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Na stronie Szybkie uruchamianie kliknij pozycję **Konfigurowanie ochrony dla VMM chmury**.
2. Na karcie **Chronionych elementów** kliknij przycisk w chmurze, który chcesz skonfigurować, a następnie kliknij kartę **Konfiguracja** .
3. W polu **obiekt docelowy** wybierz **Azure**.
4. W **Przestrzeni dyskowej konta** wybierz konto Azure miejsca do magazynowania, którego używasz do replikacji.
5. Ustawiono **Szyfruj przechowywane dane** **wyłączone**. To ustawienie określa, czy dane mają zostać zaszyfrowane replikowane między lokalnej witryny i Azure.
6. W polu **Kopiuj częstotliwość** pozostaw ustawienia domyślne. Ta wartość określa, jak często dane mają być synchronizowane między lokalizacji źródłowej i docelowej.
7. W **punkty odzyskiwania Przechowuj**pozostaw to domyślne ustawienie. Z wartością domyślną zero tylko najnowsze punkt odzyskiwania dla podstawowego maszyny wirtualnej są przechowywane na serwerze hosta replice.
8. **Częstotliwość migawek spójną z aplikacją**pozostaw to domyślne ustawienie. Ta wartość określa, jak często do tworzenia migawek. Migawki upewnij się, że aplikacje są spójna przy podejmowaniu migawki za pomocą usługi kopiowania woluminu w tle (VSS).  Jeśli ustawisz wartość, upewnij się, że jest mniejsza niż liczba punktów odzyskiwania dodatkowe, które można skonfigurować.
9. W polu **czas rozpoczęcia replikacji**określić podczas początkowej replikacji danych Azure nazwa powinna rozpoczynać się. Będzie używana strefa czasowa serwera hosta funkcji Hyper-V. Zalecamy, aby zaplanować replikacji początkowej czasie mniejszego.

    ![Ustawienia replikacji chmury](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Po zapisaniu ustawień zadanie zostanie utworzona i można monitorować na karcie **zadania** . Wszystkie serwery hosta funkcji Hyper-V w chmurze źródła VMM zostanie skonfigurowany do replikacji.

Po zapisaniu na karcie **Konfigurowanie** można modyfikować ustawienia chmury. Aby zmodyfikować lokalizacji docelowej lub konta docelowego miejsca do magazynowania, musisz usunąć Konfiguracja chmury, a następnie ponownie skonfigurować chmury. Należy zauważyć, że jeśli zmienisz konto miejsca do magazynowania zmiana jest stosowane tylko dla maszyn wirtualnych, dla których włączono ochronę po zmienieniu konta miejsca do magazynowania. Istniejące maszyn wirtualnych nie są migrowane do nowego konta miejsca do magazynowania.

## <a name="step-7-configure-network-mapping"></a>Krok 7: Konfigurowanie mapowanie sieci
Przed rozpoczęciem mapowanie sieci upewnij się, że maszyn wirtualnych na serwerze VMM źródłowym jest połączony z siecią maszyn wirtualnych. Ponadto utworzyć jeden lub więcej Azure wirtualnych sieci. Należy zauważyć, że wiele maszyn wirtualnych sieci mogą być mapowane na jednej sieci Azure.

1. Na stronie Szybkie uruchamianie kliknij przycisk **Mapuj sieci**.
2. Na karcie **sieci** w **lokalizacji źródłowej**, wybierz serwer VMM źródła. W **lokalizacji docelowej** wybierz Azure.
3. Lista maszyn wirtualnych sieci skojarzonych z serwerem VMM są wyświetlane w sieciach **źródła** . W sieci **docelowej** są wyświetlane Azure sieci skojarzone z subskrypcją.
4. Zaznacz źródło maszyn wirtualnych sieci i kliknij pozycję **Mapa**.
5. Na stronie **Wybieranie sieci docelowej** wybierz element docelowy Azure sieci, do której chcesz użyć.
6. Kliknij znacznik wyboru, aby ukończyć proces mapowania.

    ![Ustawienia replikacji chmury](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Po zapisaniu ustawień zadanie rozpoczyna się w celu śledzenia postępu mapowania i mogą być monitorowane na karcie zadania. Wszelkie istniejące maszyn wirtualnych replice, które odpowiadają źródła maszyn wirtualnych sieci będzie połączony z docelowej Azure sieci. Nowe maszyn wirtualnych, które są połączone z siecią maszyn wirtualnych źródło będzie połączony z sieci Azure zamapowanych po replikacji. Zmodyfikowanie istniejącego mapowania z nową sieć maszyn wirtualnych replice zostaną połączone za pomocą nowych ustawień.

Należy zauważyć, że jeśli sieci docelowej ma wiele podsieci i jedną z tych podsieci ma taką samą nazwę jak podsieci, na którym znajduje się maszyny wirtualnej źródła, a następnie maszyny wirtualnej replice będzie połączony z tej podsieci docelowej po przełączeniu. W przypadku nie podsieć docelowej z odpowiednia nazwa maszyny wirtualnej zostanie połączony z pierwszą podsieć w sieci.

> [AZURE.NOTE] [Migracja sieci](../resource-group-move-resources.md) wzdłuż grup zasobów w obrębie tej samej subskrypcji lub w subskrypcjach nie jest obsługiwane dla sieci używane do wdrażania Odzyskiwanie witryny.

## <a name="step-8-enable-protection-for-virtual-machines"></a>Krok 8: Włącz ochronę dla maszyn wirtualnych

Po serwerów, chmury i sieci zostały skonfigurowane poprawnie, można włączyć ochronę maszyn wirtualnych w chmurze. Pamiętaj o następujących kwestiach:

- Maszyn wirtualnych musi spełniać [wymagania Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).
- Aby włączyć ochronę systemu operacyjnego i system operacyjny właściwości dysku przeznaczona dla maszyny wirtualnej. Po utworzeniu maszyny wirtualnej w VMM przy użyciu szablonu maszyn wirtualnych można ustawić właściwość. Można także ustawić następujące właściwości istniejącego maszyn wirtualnych na kartach **Ogólne** i **Konfiguracji sprzętowej** właściwości maszyn wirtualnych. Jeśli nie ustawisz tych właściwości w VMM można skonfigurować je w portalu Odzyskiwanie witryny Azure.

    ![Tworzenie maszyn wirtualnych](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![Modyfikowanie właściwości maszyn wirtualnych](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)


1. Aby włączyć ochronę, na karcie **maszyn wirtualnych** w chmurze, w którym znajduje maszyny wirtualnej, kliknij pozycję **Włącz ochronę** > **maszyn wirtualnych Dodaj**.
2. Z listy maszyn wirtualnych w chmurze wybierz ten, który chcesz chronić.

    ![Włącz ochronę maszyn wirtualnych](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Śledzenie postępu działań **Włącz ochronę** na karcie **zadania** , w tym replikacji początkowej. Po uruchomieniu zadania **Finalizowanie ochrony** maszyny wirtualnej jest gotowy do przełączania awaryjnego. Po włączeniu ochrony i maszyn wirtualnych są replikowane, będzie można wyświetlać je w Azure.


    ![Zadania ochrony maszyn wirtualnych](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

3. Sprawdź właściwości maszyn wirtualnych, a następnie zmodyfikuj odpowiednio do potrzeb.

    ![Sprawdź maszyn wirtualnych](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)


4. Na karcie **Konfigurowanie** właściwości maszyn wirtualnych można modyfikować następujące właściwości sieci.





- **Liczba kart sieciowych na komputerze wirtualnych docelowym** — liczba kart sieciowych zależy od rozmiaru wybrane maszyny wirtualnej docelowej. Zaznacz [dane rozmiar maszyn wirtualnych](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) dla liczby obsługiwanych przez rozmiar maszyn wirtualnych kart. Możesz zmienić rozmiar dla maszyny wirtualnej i zapisać ustawienia Liczba sieciową zmienia po otwarciu strony **Konfigurowanie** następnym razem. Liczba kart sieciowych maszyn wirtualnych docelowej jest minimalna liczba kart sieciowych na komputerze wirtualnych źródła i maksymalną liczbę karty sieciowe są obsługiwane przez rozmiar maszyny wirtualnej wybrany w następujący sposób:

    - Jeśli liczba kart sieciowych na komputerze źródła jest mniejsza niż lub równa argumentowi liczby kart niedozwolone dla rozmiaru komputera docelowego, docelowej będą mieć taką samą liczbę kart jako źródła.
    - Jeśli liczba kart maszyny wirtualnej źródła przekracza dozwolony dla rozmiaru docelowego, a następnie maksymalny rozmiar docelowej zostanie użyty numer.
    - Na przykład jeśli komputer źródłowy ma dwie karty sieciowe i rozmiar komputer docelowy obsługuje cztery, komputerze docelowym ma dwie karty. Jeśli komputer źródłowy ma dwie karty, ale rozmiar obsługiwane docelowy obsługuje tylko jeden komputer docelowy będzie miał tylko jedna karta.    

- **Sieć maszyny wirtualnej docelowej** — sieci, do którego maszyna wirtualna łączy się zależy od mapowanie sieci sieci maszyn wirtualnych źródła. Jeśli maszyny wirtualnej źródła ma więcej niż jedną kartę sieciową i źródła sieci są mapowane na różnych sieci w miejscu docelowym, następnie musisz wybrać jedną z sieci docelowej.
- **Podsieć każdej karty sieciowej** - dla każdej karty sieciowej można wybrać podsieci, do której nie powiodło się nad maszyn wirtualnych chcesz nawiązać połączenie.
- **Adres IP docelowej** — Jeśli karty sieciowej maszyn wirtualnych źródła jest skonfigurowany do używania statycznego adresu IP, a następnie dla maszyny wirtualnej docelowej można określić adres IP. Ta funkcja zachować adres IP źródła maszyny wirtualnej po przełączeniu. Jeśli zostanie podany żaden adres IP wszelkie dostępne adres IP jest podany w do karty sieciowej w czasie pracy awaryjnej. Jeśli docelowy adres IP został określony, ale jest już używana przez innego komputera wirtualnych z systemem platformy Azure pracy awaryjnej wystąpi błąd.  

    ![Modyfikowanie właściwości sieci](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

>[AZURE.NOTE] Linux maszyn wirtualnych przy użyciu statycznego adresu IP nie są obsługiwane.

## <a name="test-the-deployment"></a>Testowanie rozmieszczenia

Aby przetestować wdrożenie można uruchomić trybie awaryjnym test dla jednego komputera wirtualnych lub tworzenie planu odzyskiwania składający się z wielu maszyn wirtualnych i uruchomić trybie awaryjnym test dla planu.  

Testowanie pracy awaryjnej symuluje z mechanizmu awaryjnego i odzyskiwania w odizolowanych sieci. Należy zauważyć, że:

- Jeśli chcesz połączyć się z komputerem wirtualnej platformy Azure za pomocą pulpitu zdalnego po przełączeniu włączyć Podłączanie pulpitu zdalnego maszyny wirtualnej przed uruchomieniem tym przełączeniu test.
- Po przełączeniu użyjesz publiczny adres IP połączyć się z komputerem wirtualnej platformy Azure za pomocą pulpitu zdalnego. Jeśli chcesz wykonać tę czynność, upewnij się, że nie ma żadnych zasad domeny, uniemożliwiające nawiązywanie połączenia przy użyciu publicznego adresu maszyny wirtualnej.

>[AZURE.NOTE] Aby uzyskać najlepszą wydajność, w trybie awaryjnym Azure, upewnij się, w chronionym komputerze zainstalowano agenta Azure. Ułatwia uruchamianie szybciej, a także pomaga w zakresie diagnostyki w przypadku problemów. Linux agent może być znaleziony [w tym miejscu](https://github.com/Azure/WALinuxAgent) — i Windows agenta można znaleźć [w tym miejscu](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania

1. Na karcie **Plany odzyskiwania** Dodawanie nowego planu. Określ nazwę **VMM** w polu **Typ źródła**i źródłowego serwera VMM w **źródle**, docelowej będzie Azure.

    ![Tworzenie planu odzyskiwania](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)

2. Na stronie **Wybierz pozycję maszyn wirtualnych** wybierz maszyn wirtualnych, aby dodać do planu odzyskiwania. Te maszyn wirtualnych zostaną dodane do domyślnej grupy planu odzyskiwania — grupy 1. Przetestowano maksymalnie 100 maszyn wirtualnych w planie pojedynczy odzyskiwania.

- Jeśli chcesz sprawdzić właściwości maszyn wirtualnych przed dodaniem ich do planu, kliknij pozycję maszyny wirtualnej na stronie właściwości w chmurze, w którym się znajduje. Można także skonfigurować właściwości maszyn wirtualnych w konsoli VMM.
- Wszystkie maszyn wirtualnych, które są wyświetlane włączono ochrony. Lista zawiera zarówno maszyn wirtualnych, które są włączone dla ochrony i replikacji początkowej została ukończona, i tych, które są włączone dla ochrony z pierwszym replikacji do czasu. Tylko maszyn wirtualnych z replikacji początkowej wykonane może się nie powieść nad w ramach planu odzyskiwania.

    ![Tworzenie planu odzyskiwania](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Na karcie **Plany odzyskiwania** jest wyświetlany po planu odzyskiwania utworzono go. Można również dodać [runbooks Azure automatyzacji](site-recovery-runbook-automation.md) do planu odzyskiwania, aby zautomatyzować operacje podczas pracy awaryjnej.

### <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

Istnieją dwa sposoby uruchomienia trybie awaryjnym test Azure.

- **Testowanie pracy awaryjnej bez Azure sieci**— ten rodzaj pracy awaryjnej test sprawdza, czy maszyna wirtualna pojawi się poprawnie w platformy Azure. Po przełączeniu maszyny wirtualnej nie można połączyć dowolna sieć Azure.
- **Testowanie pracy awaryjnej z siecią Azure**— tego typu kontroli pracy awaryjnej nawet zgodnie z oczekiwaniami zawiera środowisko replikacji całą i że nie powiodło się w środowisku maszyn wirtualnych systemu zostaną połączone w określonym miejscu docelowym Azure sieci. Do obsługi podsieci do badania pracy awaryjnej podsieci maszyny wirtualnej test będzie można znalezienia oparte na podsieci maszyny wirtualnej replice. Jest to różni się od regularne replikacji, gdy podsieci maszyny wirtualnej replice jest oparty na podsieci maszyny wirtualnej źródła.

Jeśli chcesz uruchomić w trybie awaryjnym test dla maszyny wirtualnej włączona funkcja Ochrona Azure bez określania sieci docelowej Azure, nie musisz nic przygotowywanie. Aby uruchomić trybie awaryjnym test z docelowej Azure sieci, musisz utworzy nową sieć Azure, który ma samodzielnie z sieci Azure produkcji (zachowanie domyślne po utworzeniu nowej sieci w Azure). Przyjrzyj się [Uruchom w trybie awaryjnym test](site-recovery-failover.md#run-a-test-failover) , aby uzyskać więcej informacji.


Musisz również skonfigurować infrastruktury zreplikowanej maszyny wirtualnej działa zgodnie z oczekiwaniami. Na przykład maszyny wirtualnej z kontrolera domeny i usługą DNS mogą być replikowane Azure za pomocą Odzyskiwanie witryny Azure i mogą być tworzone w sieci test przy użyciu Testowanie pracy awaryjnej. Przyjrzyj się sekcji [Testowanie pracy awaryjnej zagadnienia związane z usługą active directory](site-recovery-active-directory.md#considerations-for-test-failover) , aby uzyskać więcej informacji.

Aby uruchomić pracy awaryjnej test następujące czynności:

1. Na karcie **Plany odzyskiwania** wybierz plan, a następnie kliknij przycisk **Testuj pracy awaryjnej**.
2. Na stronie **Potwierdzanie pracy awaryjnej Test** wybierz pozycję **Brak** lub określonej sieci Azure.  Należy zauważyć, że wybrano Brak tym przełączeniu test sprawdza, czy poprawnie replikowane maszyny wirtualnej do Azure, ale nie sprawdza replikacji konfiguracji sieci.

    ![Bez sieci](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)

3. Po włączeniu szyfrowania danych do chmury klucza **Szyfrowania** , wybierz certyfikat, który został wydany podczas instalacji dostawcy na serwerze VMM, gdy włączona opcja włączenia szyfrowania danych do chmury.
4. Na karcie **zadania** możesz śledzić postęp pracy awaryjnej. Należy również mogli widzieć replice test maszyn wirtualnych w portalu Azure. Jeśli do maszyn wirtualnych programu access z sieci lokalnej może inicjować połączenie pulpitu zdalnego maszyn wirtualnych.
5. Po tym przełączeniu osiągnie fazy **badania wykonane** , kliknij przycisk **Testowanie wykonania** , aby zakończyć tym przełączeniu test. Możesz przejść do karty **zadania** , aby śledzić postęp pracy awaryjnej i stan i wykonywać żadnych akcji, które są potrzebne.
6. Po przełączeniu będziesz mieć możliwość Zobacz replice test maszyn wirtualnych w portalu Azure. Jeśli do maszyn wirtualnych programu access z sieci lokalnej może inicjować połączenie pulpitu zdalnego maszyn wirtualnych. Wykonaj następujące czynności:

    1. Upewnij się, że maszyn wirtualnych pomyślne uruchomienie.
    2. Jeśli chcesz połączyć się z komputerem wirtualnej platformy Azure za pomocą pulpitu zdalnego po przełączeniu włączyć Podłączanie pulpitu zdalnego maszyny wirtualnej przed uruchomieniem tym przełączeniu test. Musisz także dodać punkt końcowy RDP na maszyny wirtualnej. Można wykorzystać [Runbooks automatyzacji Azure](site-recovery-runbook-automation.md) to zrobić.
    3. Po pracy awaryjnej publiczny adres IP umożliwia łączenie maszyn wirtualnych w Azure za pomocą pulpitu zdalnego, należy się upewnić nie ma żadnych zasad domeny, uniemożliwiające nawiązywanie połączenia przy użyciu publicznego adresu maszyny wirtualnej.

7.  Po zakończeniu badania wykonaj następujące czynności:
    - Kliknij pozycję **tym przełączeniu test zostało zakończone**. Oczyść środowisku testowym automatycznego Wyłącz i Usuń maszyn wirtualnych test.
    - Kliknij **Notes** , aby nagrać, a wszelkie uwagi skojarzone z tym przełączeniu test.

>

## <a name="next-steps"></a>Następne kroki

Informacje na temat [konfigurowania planów odzyskiwania](site-recovery-create-recovery-plans.md) i [pracy awaryjnej](site-recovery-failover.md).
