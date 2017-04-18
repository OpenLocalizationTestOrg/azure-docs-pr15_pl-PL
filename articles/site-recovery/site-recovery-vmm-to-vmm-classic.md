<properties
    pageTitle="Powielić maszyn wirtualnych funkcji Hyper-V w chmury VMM do pomocniczej witryny VMM | Microsoft Azure"
    description="W tym artykule opisano, jak replikacji maszyny wirtualne funkcji Hyper-V w chmury VMM pomocniczej VMM z witryną Azure Odzyskiwanie witryny."
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
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Replikacja maszyn wirtualnych funkcji Hyper-V w chmury VMM do pomocniczej witryny VMM

> [AZURE.SELECTOR]
- [Azure portal](site-recovery-vmm-to-vmm.md)
- [Klasyczny portalu](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell — Menedżera zasobów](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Usługa Azure witryny odzyskiwania składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Urządzenia mogą być replikowane Azure lub centrum danych lokalnych pomocniczą. Krótki przegląd odczytywanie [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md)

## <a name="overview"></a>Omówienie

Ten artykuł zawiera opis sposobu replikacji maszyn wirtualnych funkcji Hyper-V na serwerach hosta funkcji Hyper-V, które są obsługiwane w chmury VMM do pomocniczej witryny VMM za pomocą Odzyskiwanie witryny Azure.

Artykuł zawiera wymagania wstępne, pokazano, jak skonfigurować magazynu Odzyskiwanie witryny, instalowanie dostawcy odzyskiwania witryny Azure na źródle docelowe VMM serwerów, rejestrowanie serwerów w magazyn, konfigurowanie ustawień ochrony dla chmur VMM i następnie włączyć ochronę maszyny wirtualne funkcji Hyper-V. Zakończyć, testując przełączanie awaryjne do upewnij się, że wszystko działa zgodnie z oczekiwaniami.

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.

## <a name="architecture"></a>Architektura

Na poniższym obrazie przedstawiono różnym kanałów i porty używane przez Odzyskiwanie witryny Azure aranżacji i replikacji

![Topologia E2E](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Przed rozpoczęciem

Upewnij się, że zostały spełnione następujące wymagania wstępne w miejscu:

**Wymagania wstępne** | **Szczegóły**
--- | ---
**Azure**| Wymagane jest konto [Microsoft Azure](https://azure.microsoft.com/) . Możesz rozpocząć z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Aby uzyskać więcej informacji](https://azure.microsoft.com/pricing/details/site-recovery/) na temat ceny Odzyskiwanie witryny.
**VMM** | Potrzebujesz co najmniej jeden serwer VMM.<br/><br/>Serwer VMM powinna działać w co najmniej systemu Centrum 2012 z dodatkiem SP1 z najnowszych aktualizacji skumulowana.<br/><br/>Jeśli chcesz skonfigurować ochrony z jednego serwera VMM konieczne co najmniej dwa chmury skonfigurowany na serwerze.<br/><br/>Jeśli chcesz wdrożyć ochronę przy użyciu dwóch serwerów VMM każdy serwer musi mieć co najmniej jeden chmury skonfigurowane na serwerze VMM podstawowy, który chcesz chronić, a jedna chmura skonfigurowany na serwerze VMM pomocniczym, który ma być używany dla ochrony i odzyskiwania<br/><br/>Wszystkie chmury VMM musi być profilu funkcji Hyper-V Ustaw.<br/><br/>W chmurze źródła, które mają być chronione musi zawierać jedną lub więcej grup hosta VMM.<br/><br/>Dowiedz się więcej o konfigurowaniu chmur VMM w [Instruktaż: tworzenie prywatnych chmury za pomocą systemu Centrum 2012 z dodatkiem SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) w blogu Jan Mayer.
**Funkcji Hyper-V** | Potrzebujesz jednego lub kilku serwerów hosta funkcji Hyper-V w grupach hosta VMM głównego i pomocniczego i co najmniej jeden maszyn wirtualnych na wszystkich serwerach hosta funkcji Hyper-V.<br/><br/>Serwery funkcji Hyper-V źródłowa i docelowa musi działać co najmniej systemu Windows Server 2012 z roli Hyper-V i zainstalowano najnowsze aktualizacje.<br/><br/>Dowolny serwer funkcji Hyper-V zawierający maszyny wirtualne mają być chronione musi znajdować się w chmurze VMM.<br/><br/>Jeśli używasz funkcji Hyper-V w klastrze, należy zauważyć, że tego brokera klaster nie jest tworzone automatycznie, jeśli masz statyczne klaster oparty na adresie IP. Musisz ręcznie skonfigurować brokera klaster. [Aby uzyskać więcej informacji](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) w Aidan Finn wpis w blogu.
**Mapowanie sieci** | Możesz skonfigurować mapowanie sieci, aby upewnić się, że zreplikowanej maszyn wirtualnych optymalnie są umieszczane na serwerach hosta funkcji Hyper-V pomocniczej po przełączeniu i że można nawiązać odpowiednie maszyn wirtualnych sieci. Jeśli nie skonfigurowano mapowanie sieci, replice maszyny wirtualne nie można połączyć dowolna sieć, po przełączeniu.<br/><br/>Aby skonfigurować mapowania sieci podczas wdrażania, upewnij się, że maszyn wirtualnych na serwerze hosta źródła funkcji Hyper-V są połączone z siecią VMM maszyn wirtualnych. Sieci powinny być połączone z sieć logiczna, który jest skojarzony z chmury. < br /<br/>W chmurze docelowej na serwerze pomocniczym VMM służące do odzyskiwania powinien mieć odpowiadające im maszyn wirtualnych sieci skonfigurowanej i być z kolei powinien on połączony z odpowiednich logiczna sieć skojarzonego z chmurą docelowej.<br/><br/>[Aby uzyskać więcej informacji](site-recovery-network-mapping.md) na temat mapowania sieci.
**Mapowanie miejsca do magazynowania** | Domyślnie podczas replikacji maszyny wirtualnej na serwerze hosta funkcji Hyper-V źródło na serwer host docelowy funkcji Hyper-V zreplikowanej dane są przechowywane w lokalizacji domyślnej wskazaną dla hosta docelowego funkcji Hyper-V w Menedżerze funkcji Hyper-V. Mieć większą kontrolę nad którym zreplikowanej dane są przechowywane można skonfigurować mapowania miejsca do magazynowania<br/><br/> Aby skonfigurować mapowania miejsca do magazynowania, musisz skonfigurować klasyfikacje miejsca do magazynowania w źródle i docelowych VMM serwerów, przed rozpoczęciem wdrażania. Aby [uzyskać więcej informacji](site-recovery-storage-mapping.md).


## <a name="step-1-create-a-site-recovery-vault"></a>Krok 1: Tworzenie magazynu Odzyskiwanie witryny

1. Zaloguj się do [Portalu zarządzania](https://portal.azure.com) na serwerze VMM, które chcesz zarejestrować.

2. Rozwiń węzeł **Usługi danych** > **Usługi odzyskiwania** i kliknij pozycję **Witryny odzyskiwania magazynu**.

3. Kliknij pozycję **Utwórz nowe** > **Szybkie tworzenie**.

4. W polu **Nazwa**wpisz przyjazną nazwę identyfikującą magazyn.

5. W **obszarze** wybierz regionu geograficznego dla magazyn. Aby sprawdzić obsługiwanych regionów Zobacz geograficznej dostępności w [Azure Szczegóły ceny odzyskiwania witryny](http://go.microsoft.com/fwlink/?LinkId=389880).

6. Kliknij przycisk **Utwórz magazynu**.

    ![Tworzenie magazynu](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Sprawdź na pasku stanu, że magazyn został utworzony. Magazyn zostaną wyświetlone na stronie głównej usługi odzyskiwania jako **aktywną** .

## <a name="step-2-generate-a-vault-registration-key"></a>Krok 2: Generuj klucz rejestru magazynu

Generowanie klucza rejestru w magazyn. Po pobraniu dostawca odzyskiwania witryny Azure i zainstalować go na serwerze VMM, aby zarejestrować serwer VMM w magazyn za pomocą tego klucza.

1. Na stronie **Usługi odzyskiwania** kliknij pozycję Magazyn, aby otworzyć stronę Szybki Start. Szybki Start można również otworzyć w dowolnym momencie za pomocą ikony.

    ![Ikona Szybki Start](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)

2. Na liście rozwijanej wybierz **między dwiema witrynami VMM lokalnego**.
3. **Przygotowywanie serwerów VMM**kliknij **plik klucza rejestru Generuj**. Plik klucza jest generowane automatycznie i obowiązuje 5 dni po jest generowany. Jeśli Azure portal nie korzysta z serwera VMM należy skopiować ten plik na serwerze.

    ![Klucz rejestru](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Krok 3: Instalowanie dostawcy odzyskiwania Azure witryny

4. Na stronie **Szybki Start** **Przygotowywanie VMM serwerów**kliknij przycisk **Pobierz Microsoft Azure odzyskiwania dostawca witryny do instalacji na serwerach VMM** uzyskać najnowszą wersję pliku instalacyjnego dostawcy.

2. Uruchom ten plik na serwerze VMM źródłowym.

    >[AZURE.NOTE] Jeśli VMM zostanie wdrożony w klastrze i instalujesz dostawcę po raz pierwszy zainstalować go na aktywny węzeł i zakończyć instalację, aby zarejestrować serwer VMM w magazyn. Następnie zainstalować dostawcę na innych węzłach. Należy zauważyć, że uaktualniania dostawcę możesz uaktualnić we wszystkich węzłach, ponieważ ta osoba może wszystkie działać w tej samej wersji dostawcy.

3. Instalator wykonuje kilka żądań i **Sprawdź wymagania wstępne dotyczące** uprawnień do Zatrzymaj usługę VMM, aby rozpocząć instalację dostawcy. Usługa VMM zostanie automatycznie uruchomiona, po zakończeniu instalacji. Jeśli przeprowadzasz instalację w klastrze VMM zostanie wyświetlony monit, aby zatrzymać roli klaster.

4. W **Witrynie Microsoft Update** można jednak aktualizacje. To ustawienie jest włączone dostawcy aktualizacje zostaną zainstalowane zgodnie z zasadami usługi Microsoft Update.

    ![Aktualizacje firmy Microsoft](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)

5. Lokalizacja instalacji jest ustawiona na ** <SystemDrive>\Program Files\Microsoft System Centrum 2012 R2\Virtual komputera Manager\bin**. Kliknij przycisk Zainstaluj, aby rozpocząć instalację dostawcy.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)

6. Po zainstalowaniu dostawcy kliknij przycisk **Zarejestruj** , aby zarejestrować serwer w magazyn.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
9. W polu **Nazwa magazynu**Sprawdź nazwę magazynu, w którym zostanie zarejestrowany serwera. Kliknij przycisk *Dalej*.

    ![Rejestracja serwera](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. W oknie **Połączenie internetowe** określ jak dostawcy na serwerze VMM łączy się z Internetem. Wybierz pozycję **Połącz z istniejących ustawień serwera proxy** , aby korzystać z domyślnych ustawień połączenia internetowego skonfigurowany na serwerze.

    ![Ustawienia internetowej](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Jeśli chcesz używać niestandardowego serwera proxy został on skonfigurowany przed rozpoczęciem instalacji dostawcy. Podczas konfigurowania ustawień serwera proxy niestandardowy, aby sprawdzić połączenie serwera proxy spowoduje uruchomienie test.
    - Używanie niestandardowej serwera proxy, czy serwer proxy domyślne wymaga uwierzytelniania, należy wprowadzić szczegóły serwera proxy, w tym adres serwera proxy oraz portu.
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
12.  W polu Wybierz **metadanych chmury Synchronizuj** czy ma być synchronizowane metadanych dla wszystkich chmur na serwerze VMM z magazynu. Ta akcja musi tylko raz wystąpić na wszystkich serwerach. Jeśli nie chcesz zsynchronizować wszystkie chmury, można pozostawić niezaznaczone to ustawienie i synchronizowanie każdego chmury pojedynczo w chmurze właściwości w konsoli VMM.

13.  Kliknij przycisk **Dalej** , aby ukończyć proces. Po zarejestrowaniu metadanych na serwerze VMM jest pobierana przez Odzyskiwanie witryny Azure. Serwer jest wyświetlany na **Serwerach VMM** > **Serwery** w magazyn.

    ![Serwery](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Instalacja wiersza polecenia

Dostawca odzyskiwania Azure witryny można również zainstalować z poziomu wiersza polecenia. Ta metoda umożliwia instalowanie dostawcy na serwerze CORE dla systemu Windows Server 2012 R2.

1. Pobierz klucz pliku i rejestracji instalacji dostawcy do folderu. Na przykład C:\ASR.
2. Zatrzymaj usługę Menedżera maszyn wirtualnych systemu Centrum
3. Wyodrębnianie Instalatora dostawcy, uruchamiając następujące polecenia w wierszu polecenia z uprawnieniami **administratora** :

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Instalowanie dostawcy, uruchamiając:

        C:\ASR> setupdr.exe /i

5. Rejestracja dostawcy, uruchamiając:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Gdzie są parametry:

 - **/Credentials**: obowiązkowe parametr, który określa lokalizację, w której znajduje się plik klucza rejestru  
 - **/FriendlyName**: parametr obowiązkowe dla nazwy serwera hosta funkcji Hyper-V, który jest wyświetlana w portalu Azure Odzyskiwanie witryny.
 - **/EncryptionEnabled**: parametr opcjonalny, który należy używać tylko w VMM do scenariusza Azure, w razie potrzeby szyfrowania maszyn wirtualnych u spoczynku platformy Azure. Upewnij się, że nazwa pliku, który ma rozszerzenie **pfx** podane.
 - **/proxyAddress**: parametr opcjonalny, który określa adres serwera proxy.
 - **/proxyport**: parametr opcjonalny, który określa port serwera proxy.
 - **/proxyUsername**: parametr opcjonalny określający nazwę użytkownika serwera Proxy (Jeśli serwer proxy wymaga uwierzytelniania).
 - **/proxyPassword**: parametr opcjonalny, który określa hasło uwierzytelniania na serwerze proxy (Jeśli serwer proxy wymaga uwierzytelniania).  

## <a name="step-4-configure-cloud-protection-settings"></a>Krok 4: Konfiguracja chmury ustawień ochrony

Po serwery VMM są zarejestrowane, można skonfigurować ustawienia ochrony chmury. Jeśli opcja **Synchronizuj dane chmur z magazyn** jest włączona, gdy zainstalowano dostawcę wszystkie chmury na serwerze VMM pojawi się na karcie **Chronionych elementów** w magazyn. Jeśli użytkownik nie może być synchronizowana określonej chmurze Odzyskiwanie witryny Azure na karcie **Ogólne** na stronie Właściwości chmury w konsoli VMM.

![Opublikowanych chmury](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Na stronie Szybkie uruchamianie kliknij pozycję **Konfigurowanie ochrony dla VMM chmury**.
2. Na karcie **Chmury VMM** wybierz chmury, którą chcesz skonfigurować, a następnie kliknij kartę **Konfiguracja** .
3. W **docelowej**zaznacz **VMM**.
4. W **lokalizacji docelowej**wybierz na miejscu serwer VMM zarządzającą w chmurze, który ma być używany do odzyskiwania.
4. W **chmurze docelowej**zaznacz w chmurze docelowej, który ma być używany do przełączania awaryjnego maszyn wirtualnych w chmurze źródła. Należy zauważyć, że:

    - Zalecamy wybranie chmurze docelowej, która spełnia wymagania odzyskiwania dla maszyn wirtualnych, które będą chronić.
    - Chmurę tylko mogą należeć do sparowania pojedynczy chmury — jako podstawowego lub w chmurze docelowej.

5. W polu **Kopiuj częstotliwość**, określić, jak często dane mają być synchronizowane między 5he lokalizacji źródłowej i docelowej. Należy zauważyć, że to ustawienie dotyczy tylko po host funkcji Hyper-V jest uruchomiony Windows Server 2012 R2. W przypadku innych serwerów używane domyślne ustawienia pięć minut.
6. W **odzyskiwania dodatkowe punkty**Określ, czy chcesz utworzyć odzyskiwania dodatkowe punkty. Wartość domyślna zero wskazuje, że tylko najnowsze punkt odzyskiwania dla podstawowego maszyny wirtualnej są przechowywane na serwerze hosta replice. Należy zauważyć, że włączenie wiele punktów odzyskiwania wymaga dodatkowego miejsca do magazynowania dla migawki, które są przechowywane w każdym punkcie odzyskiwania. Domyślnie punkty odzyskiwania są tworzone co godzinę, dzięki czemu każdego punktu odzyskiwania zawiera wartość godziny danych. Wartość punktu odzyskiwania, przypisywane maszyny wirtualnej w konsoli VMM nie powinna być mniejsza niż wartość przypisywanie w konsoli odzyskiwania witryny Azure.
7. **Częstotliwość migawek spójnych aplikacji**określ częstotliwość tworzenia migawek spójnych aplikacji. Funkcji Hyper-V używa dwóch typów migawek — standardowy migawkę zawierającego przyrostowe migawki całego maszyny wirtualnej i migawkę spójną z aplikacją, która pobiera migawkę w chwili dane aplikacji wewnątrz maszyny wirtualnej. Migawek spójnych aplikacji upewnij się, że aplikacje są spójna przy podejmowaniu migawki za pomocą usługi kopiowania woluminu w tle (VSS). Należy zauważyć, że po włączeniu migawek spójnych aplikacji wpłynie to na wydajność aplikacji uruchomionych maszyn wirtualnych źródła. Upewnij się, że ustawioną wartość jest mniejsza niż liczba punktów odzyskiwania dodatkowe, które można skonfigurować.

    ![Konfigurowanie ustawień ochrony](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)

8. W polu **kompresji przesyłania danych**Określ, czy być kompresowany zreplikowanej dane, które są przenoszone.
9. W przypadku **uwierzytelniania**Określ, jak ruch uwierzytelnieniu się między serwerami hosta podstawowego i odzyskiwania funkcji Hyper-V. Wybierz pozycję HTTPS, chyba że skonfigurowane środowisko Kerberos działa. Odzyskiwanie witryny Azure automatycznie skonfiguruje certyfikatów do uwierzytelniania HTTPS. Konfiguracja ręczna, nie jest wymagane. Jeśli wybierzesz Kerberos, bilet Kerberos będzie używana do wzajemnego uwierzytelniania serwerów hosta. Domyślnie port 8083 i 8084 (w przypadku certyfikaty) zostanie otwarty w Zaporze systemu Windows na serwerach hosta funkcji Hyper-V. Zauważ, że to ustawienie ma zastosowanie tylko dla funkcji Hyper-V hosta serwery mają zainstalowany w systemie Windows Server 2012 R2.
10. W polu **Port**zmodyfikować numer portu, na którym hostów źródłowej i docelowej nasłuchują ruchu replikacji. Na przykład zmodyfikować ustawienie, jeśli chcesz zastosować jakości z usług sieci przepustowości ruch replikacji. Sprawdź, czy port nie jest używany przez inną aplikację i że jest otwarty w ustawieniach zapory.
11. W polu **Metoda replikacji**Określ sposób replikacji początkowej danych ze źródła do lokalizacji docelowej obsługi, przed rozpoczęciem regularnie replikacji:

    - **Sieci**— kopiowanie danych przez sieć może być czasochłonne i zasobów. Zalecamy, użyj tej opcji chmury zawiera maszyn wirtualnych ze stosunkowo niewielką wirtualnych dyskach twardych, a podstawowej witryny jest połączony pomocniczej witryny przez połączenie z wielu przepustowości. Można określić, że kopia należy natychmiast rozpocząć lub wybierz czas. Jeśli replikacji sieci zalecamy planowanie czasie mniejszego.
    - **Tryb offline**— ta metoda określa, że replikacji początkowej będzie można wykonać przy użyciu nośników zewnętrznych. Okazuje się przydatne, jeśli chcesz uniknąć obniżenie wydajności sieci lub geograficznie zdalnego lokalizacji. Aby użyć tej metody Określ lokalizację eksportu w chmurze źródła i Importowanie lokalizacji w chmurze docelowej. Po włączeniu ochrony dla maszyny wirtualnej wirtualny dysk twardy jest kopiowana do lokalizacji określonej dla eksportu. Wyślij go do witryny docelowej, a następnie skopiować go do lokalizacji importu. Importowanymi informacjami jest kopiowany do maszyn wirtualnych replice.

12. Wybierz pozycję **Usuń maszyn wirtualnych replice** Aby określić, że maszyny wirtualnej replice powinny zostać usunięte, jeśli przestaniesz ochrona maszyny wirtualnej przez wybranie opcji **Usuń ochrony dla maszyny wirtualnej** na karcie maszyn wirtualnych właściwości chmury. To ustawienie jest włączona, po wyłączeniu ochrony maszyny wirtualnej zostanie usunięta z Odzyskiwanie witryny Azure, ustawienia Odzyskiwanie witryny dla maszyny wirtualnej są usuwane w konsoli VMM i replice zostanie usunięty.

    ![Konfigurowanie ustawień ochrony](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Po zapisaniu ustawień zadanie zostanie utworzona i można monitorować na karcie **zadania** . Wszystkie serwery hosta funkcji Hyper-V w chmurze źródła VMM zostanie skonfigurowany do replikacji. Na karcie **Konfigurowanie** można modyfikować ustawienia chmury. Jeśli chcesz zmodyfikować w chmurze docelowej lub lokalizacji docelowej należy usunąć Konfiguracja chmury, a następnie ponownie skonfigurować chmury.

### <a name="prepare-for-offline-initial-replication"></a>Przygotowywanie do trybu offline replikacji początkowej

Musisz wykonać następujące czynności, aby przygotować się do trybu offline replikacji początkowej:

- Na serwerze źródłowym będzie Określ lokalizacji, z której Eksport danych ma być wykonywana. Pełna kontrola przypisywanie uprawnień NTFS i udostępnianie z usługą VMM na ścieżce eksportu. Na serwerze docelowym będzie Określ lokalizacji, z której nastąpi importowania danych. Przypisywanie takie same uprawnienia na następującą ścieżkę importu.
- Jeśli ścieżka importu lub eksportu jest udostępniony, przypisz administratora, użytkownicy zaawansowani, Operator wydruku lub Operator serwera członkostwa w grupach dla konta usługi VMM na komputerze zdalnym, na którym udostępnionego znajduje się.
- Jeśli dodawanie hostów na ścieżce importowania i eksportowania przy użyciu dowolnego konta Uruchom jako przypisywanie odczytu i zapisu rachunku Uruchom jako VMM.
- Importowanie i eksportowanie akcji nie powinien znajdować się na dowolnym komputerze używany jako serwer hosta funkcji Hyper-V, ponieważ konfiguracja sprzężenia zwrotnego nie jest obsługiwana przez funkcji Hyper-V.
- W usłudze Active Directory na poszczególnych funkcji Hyper-V hosta serwera, który zawiera maszyn wirtualnych, który chcesz chronić, włączanie i konfigurowanie delegowanie ograniczeń za zaufany komputerów zdalnych, na których ścieżki importowania i eksportowania znajdują się, w następujący sposób:
    1. Na kontrolerze domeny otwórz **Użytkownicy usługi Active Directory i komputery**.
    2. W drzewie konsoli kliknij **nazwa_domeny** > **komputerach**.
    3. Kliknij prawym przyciskiem myszy nazwę serwera funkcji Hyper-V > **Właściwości**.
    4. Na karcie **Delegowanie** kliknij T**Rdza ten komputer delegowanie tylko do określonych usług**.
    5. Kliknij opcję **Użyj dowolnego protokołu uwierzytelniania**.
    6. Kliknij przycisk **Dodaj** > **użytkowników i komputerów**.
    7. Wpisz nazwę komputera obsługującego ścieżki eksportu > **OK**. Z listy dostępnych usług, przytrzymaj naciśnięty klawisz CTRL, a następnie kliknij pozycję **cifs** > **przycisk OK**. Powtórz nazwę komputera obsługującego ścieżka importu. Powtórz stosownie do potrzeb dodatkowe serwery hosta funkcji Hyper-V.

## <a name="step-5-configure-network-mapping"></a>Krok 5: Konfigurowanie mapowanie sieci
1. Na stronie Szybkie uruchamianie kliknij przycisk **Mapuj sieci**.
2. Wybierz serwer VMM źródło, z którego chcesz zamapować sieci, a następnie na docelowym serwerze VMM, do którego będą mapowane sieci. Lista źródła sieci i ich sieci docelowej skojarzone są wyświetlane. Wartość pusta jest wyświetlana dla sieci, które nie są obecnie mapowane.
3. Wybieranie sieci w **sieci na źródle** > **mapy**. Usługa wykrywa sieci maszyn wirtualnych na serwerze docelowym i wyświetla je. Kliknij ikonę informacji obok nazwy sieci źródłowej i docelowej, aby wyświetlić podsieci dla każdej sieci.

    ![Konfigurowanie mapowanie sieci](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)

4. W oknie dialogowym wybierz jedną z sieci maszyn wirtualnych na serwerze VMM docelowej.

    ![Wybieranie sieci docelowej](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)

5. Po wybraniu sieci docelowej chronionego chmury, które korzystają z sieci źródła są wyświetlane. Wyświetlane są również dostępne docelowej sieci, z którymi są skojarzone z chmury używane do ochrony. Zalecamy, wybierz pozycję sieci docelowej, które są dostępne dla wszystkich chmur, używanym do ochrony. Lub można również przejść do serwera VMM i modyfikować właściwości chmurze w celu dodania sieci logiczne odpowiadające maszyn wirtualnych sieci, dla której chcesz wybrać.
6. Kliknij znacznik wyboru, aby ukończyć proces mapowania. Zadanie rozpoczyna się w celu śledzenia postępu mapowania. Na karcie **zadania** można wyświetlić go.


## <a name="step-6-configure-storage-mapping"></a>Krok 6: Skonfiguruj mapowanie miejsca do magazynowania
Domyślnie podczas replikacji maszyny wirtualnej na serwerze hosta funkcji Hyper-V źródło na serwer host docelowy funkcji Hyper-V zreplikowanej dane są przechowywane w lokalizacji domyślnej wskazaną dla hosta docelowego funkcji Hyper-V w Menedżerze funkcji Hyper-V. Mieć większą kontrolę nad którym zreplikowanej dane są przechowywane możesz skonfigurować mapowania miejsca do magazynowania w następujący sposób:



1. Definiowanie klasyfikacje miejsca do magazynowania na serwerach VMM źródłowej i docelowej. Aby [uzyskać więcej informacji](https://technet.microsoft.com/library/gg610685.aspx). Klasyfikacje musi być dostępny do serwerów głównych funkcji Hyper-V w źródłowej i docelowej chmury. Klasyfikacje nie muszą być tego samego typu miejsca do magazynowania. Na przykład można mapować Klasyfikacja źródła, która zawiera udziały SMB w celu klasyfikacji docelowej, która zawiera CSVs.
2. Po klasyfikacje w miejscu możesz utworzyć mapowania. W tym celu na stronie **Szybki Start** > **Mapa miejsca do magazynowania**.
3. Kliknij kartę **miejsca do magazynowania** > **Mapa klasyfikacje miejsca do magazynowania**.
4. Na karcie **Mapowanie klasyfikacje miejsca do magazynowania** wybierz pozycję klasyfikacje w źródle i docelowych VMM serwerów. Zapisywanie ustawień.

    ![Wybieranie sieci docelowej](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)


## <a name="step-7-enable-virtual-machine-protection"></a>Krok 7: Włącz ochronę maszyn wirtualnych
Po serwerów, chmury i sieci zostały skonfigurowane poprawnie, można włączyć ochronę maszyn wirtualnych w chmurze.

1. Na karcie **maszyn wirtualnych** w chmurze, w którym znajduje się maszyny wirtualnej, kliknij przycisk **Włącz ochronę** > **maszyn wirtualnych Dodaj**.
2. Z listy maszyn wirtualnych w chmurze wybierz ten, który chcesz chronić.

    ![Włącz ochronę maszyn wirtualnych](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)

3. Śledzenie postępu działań Włącz ochronę na karcie **zadania** , w tym replikacji początkowej. Po uruchomieniu zadania finalizowanie ochrony maszyny wirtualnej jest gotowy do przełączania awaryjnego. Po włączeniu ochrony i maszyn wirtualnych są replikowane, będzie można wyświetlać je w Azure.

    ![Zadania ochrony maszyn wirtualnych](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

>[AZURE.NOTE] Można także włączyć ochronę maszyn wirtualnych w konsoli VMM. Kliknij przycisk **Włącz ochronę** na pasku narzędzi na karcie **Odzyskiwanie witryny Azure** we właściwościach maszyn wirtualnych.

### <a name="on-board-existing-virtual-machines"></a>Płycie istniejących maszyn wirtualnych

Jeśli masz istniejące maszyn wirtualnych w VMM, które są replikowane przy użyciu funkcji Hyper-V replice musisz płycie ich ochrony Odzyskiwanie witryny Azure w następujący sposób:

1. Sprawdź, czy masz głównego i pomocniczego chmury. Upewnij się, że serwer funkcji Hyper-V obsługujący istniejące maszyny wirtualnej znajduje się w chmurze podstawowego i że serwera funkcji Hyper-V hostingu maszyny wirtualnej replice znajduje się w chmurze pomocniczą. Upewnij się, że skonfigurowano ustawienia ochrony dla chmury. Ustawienia powinny się zgadzać obecnie skonfigurowane dla funkcji Hyper-V replice. W przeciwnym razie replikacji maszyna wirtualna może nie działać zgodnie z oczekiwaniami.
2. Następnie włącz ochronę podstawowego maszyny wirtualnej. Azure Odzyskiwanie witryny i VMM będziesz mieć pewność, że zostanie wykryta samego hosta replice i maszyn wirtualnych i odzyskiwanie witryny Azure będzie ponowne używanie przywrócić replikacji przy użyciu ustawień skonfigurowanych podczas konfigurowania chmury.


## <a name="test-your-deployment"></a>Testowanie wdrożenia

Aby przetestować wdrożenie można uruchomić trybie awaryjnym test dla jednego komputera wirtualnych lub tworzenie planu odzyskiwania składający się z wielu maszyn wirtualnych i uruchomić trybie awaryjnym test dla planu.  Testowanie pracy awaryjnej symuluje z mechanizmu awaryjnego i odzyskiwania w odizolowanych sieci.

### <a name="create-a-recovery-plan"></a>Tworzenie planu odzyskiwania

1. Na karcie **Plany odzyskiwania** kliknij polecenie **Utwórz Plan odzyskiwania**.
2. Określ nazwę planu odzyskiwania i serwery VMM źródłowej i docelowej. Serwer źródłowy musi mieć maszyn wirtualnych, które są włączone dla awaryjnego i odzyskiwania. Wybierz pozycję **Funkcji Hyper-V** , aby wyświetlić tylko chmury, które są skonfigurowane dla funkcji Hyper-V replikacji.

    ![Tworzenie planu odzyskiwania](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)

3. **Wybierz pozycję maszyn wirtualnych**wybierz grupy replikacji. Wszystkie maszyn wirtualnych skojarzonych z grupą replikacji zostaną zaznaczone i dodane do planu odzyskiwania. Te maszyn wirtualnych zostaną dodane do domyślnej grupy planu odzyskiwania — grupy 1. w razie potrzeby możesz dodać więcej grup. Zauważ, że po maszyn wirtualnych replikacji rozpocznie zgodnie z kolejnością grup plan odzyskiwania.

    ![Dodawanie maszyn wirtualnych](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Po odzyskiwania plan został utworzony, wydaje się na liście na karcie **Plany odzyskiwania** .

###<a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

1. Na karcie **Plany odzyskiwania** wybierz plan, a następnie kliknij przycisk **Testuj pracy awaryjnej**.
2. Na stronie **Potwierdzanie pracy awaryjnej Test** wybierz opcję **Brak**. Należy zauważyć, że ta opcja jest włączone nie powiodło się maszyn wirtualnych replice nie będzie podłączony do każdej sieci. Spowoduje to przetestowanie, że maszyny wirtualnej przełącza zgodnie z oczekiwaniami, ale nie sprawdza środowiska sieciowego replikacji. Przyjrzyj się [Uruchom w trybie awaryjnym test](site-recovery-failover.md#run-a-test-failover) , aby uzyskać więcej informacji o używaniu różne opcje sieci.
3. Test maszyny wirtualnej zostanie utworzony na tym samym hoście jako host, na którym istnieje maszyny wirtualnej replice. Dodaje się w chmurze samej, w którym znajduje się maszyny wirtualnej replice.

### <a name="run-a-recovery-plan"></a>Uruchamianie planu odzyskiwania
Po replikacji maszyny wirtualnej replice nie może być adres IP, który jest taki sam, jak adres IP podstawowego maszyny wirtualnej. Maszyn wirtualnych zaktualizuje serwera DNS, które używają po rozpoczęciu. Można również dodać skrypt, aby zaktualizować serwer DNS, aby zapewnić czas aktualizacji.

#### <a name="script-to-retrieve-the-ip-address"></a>Skrypt, aby pobrać adres IP
Uruchom ten przykładowy skrypt, aby pobrać adresu IP.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Skrypt umożliwiający aktualizowanie DNS
Uruchom ten przykładowy skrypt, aby zaktualizować system DNS, określając adres IP, który można pobrać za pomocą przykładowy skrypt powyżej.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Informacje o prywatności dotyczące Odzyskiwanie witryny

Ta sekcja zawiera informacje o dodatkowe prywatności dotyczące Odzyskiwanie witryny Microsoft Azure usługa (""). Aby wyświetlić zasady zachowania poufności informacji dla usług Microsoft Azure, zobacz [Microsoft Azure — zasady zachowania poufności informacji](http://go.microsoft.com/fwlink/?LinkId=324899)

**Funkcja: rejestracji**

- **Co oznacza**: rejestruje serwera za pomocą usługi, tak aby maszyn wirtualnych mogą być chronione
- **Informacje zbierane**: po rejestrowania usługi zbiera przetwarza i przesyła informacje o zarządzaniu certyfikat z serwera VMM, która wyznaczyła zapewnienie awarii przy użyciu usługi nazwa serwera VMM i nazwę chmur maszyn wirtualnych na serwerze VMM.
- **Wykorzystywanie informacji**:
    - Certyfikat zarządzania — to służy do identyfikowania i uwierzytelniania serwera VMM zarejestrowanych w celu uzyskiwania dostępu do usługi. Usługa używa części publicznej klucza certyfikatu do zabezpieczenia tokenu tylko zarejestrowanych serwer VMM mogą uzyskiwać dostęp do. Serwer wymaga token ten umożliwia uzyskanie dostępu do funkcji usługi.
    - Nazwa serwera VMM — VMM nazwa serwera jest wymagana do identyfikowania i komunikować się z odpowiedniego serwera VMM, na którym znajdują się chmury.
    - Nazwy z serwerem VMM w chmurze — wymagana jest nazwa chmury, korzystając z chmury usługi sparowania unpairing funkcji opisane poniżej. Jeśli jednak chcesz skojarzyć z chmury centrum danych podstawowych z innego chmury w centrum danych odzyskiwania, nazwy wszystkich chmur z centrum danych odzyskiwania są prezentowane.

- **Wybór**: te informacje są podstawowe części proces rejestracji usługi, ponieważ ułatwia możesz i usługa identyfikowania serwera VMM, dla której ma zostać chroniące Odzyskiwanie witryny Azure, a także aby określić poprawny serwer VMM zarejestrowanych. Jeśli nie chcesz wysyłać te informacje do usługi, należy używać tej usługi. Jeśli rejestrowanie serwera, a później zostać następnie unregister go, możesz to zrobić, usuwając informacje o serwerze VMM z portalu usługi (czyli Azure portal).

**Funkcja: Włącz ochronę Odzyskiwanie witryny Azure**

- **Co oznacza**: Dostawca Azure witryny odzyskiwania, zainstalowane na serwerze VMM jest kanał do komunikowania się z usługą. Dostawca jest obsługiwany w procesie VMM biblioteki dołączanej (dynamicznie DLL). Po zainstalowaniu dostawcy w konsoli administratora VMM otrzymuje włączyć funkcję "Odzyskiwania centrum danych". Wszelkie maszyn wirtualnych nowym lub istniejącym w chmurze, aby umożliwić właściwość o nazwie "Centrum danych odzyskiwania" chronić maszyny wirtualnej. Po ustawieniu tej właściwości dostawca wysyła nazwy i Identyfikatora maszyny wirtualnej z usługą. Ochrona wirtualna jest włączona przez technologię replikacji systemu Windows Server 2012 lub Windows Server 2012 R2 Hyper-V. Dane maszyn wirtualnych otrzymuje replikowane z jednego hosta funkcji Hyper-V do innego (zwykle znajduje się w centrum danych, różnych "odzyskiwania").

- **Informacje zbierane**: usługa zbiera, przetwarza i przesyła metadanych dla maszyny wirtualnej, co zawiera nazwę, identyfikator, wirtualną sieć i nazwę chmury do której należy.

- **Wykorzystywanie informacji**: usługa używa powyższych informacji można wypełnić informacje maszyn wirtualnych w portalu usługi sieci.

- **Wybór**: to jest podstawowe częścią usługi i nie można wyłączyć. Jeśli nie chcesz, aby te informacje wysyłane do usługi, nie Włącz ochronę Odzyskiwanie witryny Azure dla dowolnego maszyn wirtualnych. Należy zauważyć, że wszystkie dane wysyłane przez dostawcę usługi jest wysyłana za pomocą protokołu HTTPS.

**Funkcja: Planowanie odzyskiwania**

- **Co oznacza**: Ta funkcja pomaga utworzyć plan aranżacji centrum danych "odzyskiwania". Można określić kolejność, w której maszyn wirtualnych lub grupę maszyn wirtualnych powinny być uruchamiane w witrynie odzyskiwania. Można również określić skrypty automatyczną ma być akcją wykonywania lub dowolną ręczny podejmowane podczas odzyskiwania dla każdej maszyny wirtualnej. Pracy awaryjnej (objęte w następnej sekcji), zazwyczaj zostanie wywołana na poziomie planowanie odzyskiwania skoordynowanego odzyskiwania.

- **Informacje zbierane**: Usługa zbierane, przetwarzane i przesyła metadanych dla planu odzyskiwania, w tym metadanych maszyn wirtualnych i metadanych skryptów automatyzacji i notatki ręczne akcji.

- **Wykorzystywanie informacji**: metadane opisany powyżej służy do tworzenia planu odzyskiwania w portalu usługi.

- **Wybór**: to jest podstawowe częścią usługi i nie można wyłączyć. Jeśli nie chcesz, aby te informacje wysyłane do usługi, nie tworzyć odzyskiwania plany w tej usłudze.

**Funkcja: Mapowanie sieci**

- **Co oznacza**: Ta funkcja umożliwia mapowanie informacji dotyczących sieci z poziomu Centrum danych podstawowych centrum danych odzyskiwania. Jeśli maszyn wirtualnych odzyskuje się w witrynie odzyskiwania to mapowanie pomaga w ustalaniu łączność sieciowa dla nich.

- **Informacje zbierane**: jako część funkcji mapowania sieci, usługa zbiera, przetwarza i przesyła metadane sieci logicznych dla każdej witryny (podstawowego i centrum danych).

- **Wykorzystywanie informacji**: usługa używa metadanych do wypełnienia do portalu miejsce, w którym można mapować informacje o sieci.

- **Wybór**: to jest podstawowe częścią usługi i nie można wyłączyć. Jeśli nie chcesz, aby te informacje wysyłane do usługi, nie używaj funkcji mapowania sieci.

**Funkcja: Pracy awaryjnej - planowanej niezaplanowane, test**

- **Co oznacza**: Ta funkcja umożliwia przejęcie awaryjne maszyny wirtualnej z jednym centrum danych zarządzanych VMM innym centrum danych zarządzanych VMM. Akcja pracy awaryjnej zostanie wywołana przez użytkownika na ich portalu. Możliwe przyczyny trybie awaryjnym zawiera zdarzenie niezaplanowane (na przykład w przypadku disaster0 naturalne; planowanych zdarzeń (na przykład centrum danych równoważenia obciążenia); w trybie awaryjnym test (na przykład odzyskiwania plan próba).

Dostawcy na serwerze VMM otrzymuje powiadomienie zdarzenia przez usługę i wykonuje akcję pracy awaryjnej na hoście funkcji Hyper-V za pośrednictwem interfejsów VMM. Rzeczywisty awaryjnego przeniesienia maszyny wirtualnej z jednego hosta funkcji Hyper-V do innego (zwykle działa w centrum danych różnych "odzyskiwania") jest obsługiwany przez technologia replikacji systemu Windows Server 2012 lub Windows Server 2012 R2 Hyper-V. Po zakończeniu tym przełączeniu dostawcy zainstalowanego na serwerze VMM centrum danych "odzyskiwania" przesyła informacje sukcesu z usługą.

- **Informacje zbierane**: usługa używa powyższych informacji można wypełnić stanu informacji o akcji pracy awaryjnej w portalu usługi sieci.

- **Wykorzystywanie informacji**: usługa używa powyższych informacji w następujący sposób:

    - Certyfikat zarządzania — to służy do identyfikowania i uwierzytelniania serwera VMM zarejestrowanych w celu uzyskiwania dostępu do usługi. Usługa używa części publicznej klucza certyfikatu do zabezpieczenia tokenu tylko zarejestrowanych serwer VMM mogą uzyskiwać dostęp do. Serwer wymaga token ten umożliwia uzyskanie dostępu do funkcji usługi.
    - Nazwa serwera VMM — VMM nazwa serwera jest wymagana do identyfikowania i komunikować się z odpowiedniego serwera VMM, na którym znajdują się chmury.
    - Nazwy z serwerem VMM w chmurze — wymagana jest nazwa chmury, korzystając z chmury usługi sparowania unpairing funkcji opisane poniżej. Jeśli jednak chcesz skojarzyć z chmury centrum danych podstawowych z innego chmury w centrum danych odzyskiwania, nazwy wszystkich chmur z centrum danych odzyskiwania są prezentowane.

- **Wybór**: to jest podstawowe częścią usługi i nie można wyłączyć. Jeśli nie chcesz, aby te informacje wysyłane do usługi, nie używaj tej usługi.

## <a name="next-steps"></a>Następne kroki

Po uruchomieniu awarię test na Sprawdź, czy środowiska działa zgodnie z oczekiwaniami, [Dowiedz się więcej o](site-recovery-failover.md) różnych rodzajów praca awaryjna.
