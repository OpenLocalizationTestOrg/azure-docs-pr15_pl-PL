<properties
    pageTitle="Pracy awaryjnej w Odzyskiwanie witryny | Microsoft Azure" 
    description="Odzyskiwanie witryny Azure współrzędne replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Informacje na temat przełączanie awaryjne do Azure lub pomocnicza centrum danych." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="failover-in-site-recovery"></a>Pracy awaryjnej w Odzyskiwanie witryny

Usługa Azure witryny odzyskiwania składa się na strategii odzyskiwania (BCDR) ciągłości i danych biznesowych przez orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych i serwerów fizycznych. Urządzenia mogą być replikowane Azure lub centrum danych lokalnych pomocniczą. Krótki przegląd odczytywanie [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md)

## <a name="overview"></a>Omówienie

Ten artykuł zawiera informacje i instrukcje dotyczące odzyskiwanie (niepowodzenie powyżej i w przypadku braku wstecz) maszyn wirtualnych i fizycznych serwerów, które są chronione Odzyskiwanie witryny. 

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)w witrynie.


## <a name="types-of-failover"></a>Typy pracy awaryjnej

Po włączeniu ochrony dla maszyn wirtualnych i serwerów fizycznych i są one replikacji praca awaryjna może zostać uruchomiony w razie potrzeby. Odzyskiwanie witryny obsługuje wiele typów pracy awaryjnej.

**Praca awaryjna** | **Kiedy należy uruchomić** | **Szczegóły** | **Proces**
---|---|---|---
**Testowanie pracy awaryjnej** | Uruchom, aby sprawdzić poprawność strategii replikacji lub wykonanie rozwijania odzyskiwania po awarii | Brak utraty danych i przestoje.<br/><br/>Nie wpływu na replikacji<br/><br/>Nie wpływu na środowisku produkcyjnym | Rozpoczynanie tym przełączeniu<br/><br/>Określ, jak będzie połączony komputerów testowych z sieci po przełączeniu<br/><br/>Śledzenie postępu na karcie **zadania** . Test są tworzone i rozpocząć w lokalizacji pomocniczej<br/><br/>Azure - połączyć się z komputerem w portalu Azure<br/><br/>Pomocniczy witryny — dostęp do komputera, na tę samą hosta i chmury<br/><br/>Wykonywanie testów i automatycznie oczyścić test ustawień pracy awaryjnej.
**Planowane pracy awaryjnej** | Uruchom, aby spełnić wymagania dotyczące zgodności<br/><br/>Uruchamianie dla planowanej konserwacji<br/><br/>Uruchamianie niepowodzenie nad danymi, aby zachować obciążenia uruchomiony dla znane dostawie — na przykład awarii zasilania oczekiwanych lub trudnych pogody<br/><br/>Uruchamianie do awarii po przełączeniu z podstawowego do pomocniczej | Bez utraty danych<br/><br/>Przestoje jest poniesione w czasie, gdy trwa zamknięcia maszyn wirtualnych w podstawowych i wyświetlić go w lokalizacji pomocniczej.<br/><br/>Maszyn wirtualnych są wpływ na komputerach docelowych będzie maszyn źródła po przełączeniu. | Rozpoczynanie tym przełączeniu<br/><br/>Śledzenie postępu na karcie **zadania** . Źródło maszyny są zamknięte.<br/><br/>Rozpocznij maszyn replice w lokalizacji pomocniczej<br/><br/>Azure - połączyć się z komputerem replice w portalu Azure<br/><br/>Pomocniczy witryny — dostęp do komputera, na tym samym hoście i w chmurze samej<br/><br/>Zatwierdź tym przełączeniu
**Nieplanowanego przełączania awaryjnego** | Uruchom ten rodzaj pracy awaryjnej w sposób reaktywne podstawowej witryny stał się niedostępne ze względu na nieoczekiwane zdarzenie, na przykład ataki awarii lub wirusami power <br/><br/> Można uruchamiać niezaplanowane pracy awaryjnej będzie możliwe, nawet jeśli podstawowej witryny nie jest dostępna. | Utratą danych zależy od ustawień częstotliwość replikacji<br/><br/>Dane są aktualne zgodnie z ostatniego została zsynchronizowana | Rozpoczynanie tym przełączeniu<br/><br/>Śledzenie postępu na karcie **zadania** . Opcjonalnie spróbuj zamknąć maszyn wirtualnych i synchronizowanie najnowszych danych<br/><br/>Rozpocznij maszyn replice w lokalizacji pomocniczej<br/><br/>Azure - połączyć się z komputerem replice w portalu Azure<br/><br/>Dostęp do witryny pomocniczą na tym samym hoście i w chmurze tym samym komputerze<br/><br/>Zatwierdź tym przełączeniu


Typy praca awaryjna, które są obsługiwane zależą od aktualnego scenariusza wdrożenia.

**Kierunek pracy awaryjnej** | **Testowanie pracy awaryjnej** | **Planowane pracy awaryjnej** | **Nieplanowanego przełączania awaryjnego**
---|---|---|---
Podstawowy witryny VMM do witryny VMM pomocniczej | Obsługiwane | Obsługiwane | Obsługiwane 
Pomocniczy witryny VMM do witryny VMM podstawowego | Obsługiwane | Obsługiwane | Obsługiwane
Chmura w chmurze (pojedynczy VMM server) |  Obsługiwane | Obsługiwane | Obsługiwane
Witryna VMM Azure | Obsługiwane | Obsługiwane | Obsługiwane 
Azure do witryny VMM | Nieobsługiwane | Obsługiwane | Nieobsługiwane 
Witryna funkcji Hyper-V Azure | Obsługiwane | Obsługiwane | Obsługiwane
Azure do witryny funkcji Hyper-V | Nieobsługiwane | Obsługiwane | Nieobsługiwane
Witryna VMware Azure | Obsługiwane (scenariusz rozszerzone)<br/><br/> Nieobsługiwane (scenariusz starsze) |Nieobsługiwane | Obsługiwane
Fizyczny serwer Azure | Obsługiwane (scenariusz rozszerzone)<br/><br/> Nieobsługiwane (scenariusz starsze) | Nieobsługiwane | Obsługiwane

## <a name="failover-and-failback"></a>Awaryjnego i powrotu

Niepowodzenie w środowisku maszyn wirtualnych systemu do witryny pomocniczej lokalnego lub Azure, w zależności od wdrożenia. Urządzenie, które nie powiedzie się nad Azure jest tworzona jako Azure maszyn wirtualnych. Na jednym komputerze wirtualnych lub serwera fizycznego lub plan odzyskiwania może się nie powieść. Plany odzyskiwania składa się z jedną lub więcej zamówiono grup, które zawierają chronionego maszyn wirtualnych lub serwerów fizycznych. Są używane do dodać akompaniament pracy awaryjnej wielu urządzeń, które nie nad według grupy się znajdują. [Dowiedz się więcej](site-recovery-create-recovery-plans.md) o planach odzyskiwania. 

Po zakończeniu pracy awaryjnej i komputery są i rozpocząć pracę w lokalizacji pomocniczej zauważyć, że:

- Jeśli możesz nie powiodło się nad Azure, po maszyn pracy awaryjnej nie są chronione, czy replikujących w lokalizacji podstawowym lub pomocniczym. Platformy Azure maszyn wirtualnych są przechowywane w magazynie replikowane geo, która zapewnia elastyczność, ale nie replikacji.
- Jeśli zarejestrowano nieplanowanego przełączania awaryjnego pomocniczej witryny po maszyn pracy awaryjnej w lokalizacji pomocniczej nie są chronione lub replikacji.
- Jeśli zarejestrowano planowane przełączanie awaryjne do witryny pomocniczej po maszyn pracy awaryjnej w lokalizacji pomocniczej jest chroniony.
 

### <a name="failback-from-secondary-site"></a>Awarii z witryny pomocniczej

Awarii z pomocniczej witryny do głównego używa tego samego procesu jako awaryjne z podstawowych do pomocniczą. Po gwarantowanej i wykonania awaryjne z podstawowego pomocniczej centrum danych można zainicjować odwrotnej replikacji witryny podstawowy stał się dostępny. Replikacja odwrotnej inicjuje replikacji między pomocniczej witryny i podstawowych przez synchronizowanie danych różnicy. W obecnym stanie odwrotnej replikacji sobie maszyn wirtualnych, ale pomocniczej centrum danych jest nadal aktywne lokalizacji. Aby nawiązać podstawowej witryny w lokalizacji aktywnego zainicjować planowane awaryjne z pomocniczej na podstawową, następuje inna replikacja odwrotnej.

### <a name="failback-from-azure"></a>Awarii z platformy Azure

Jeśli zostały powiodła się nad Azure maszyn wirtualnych są chronione przez funkcje Azure elastyczności dla maszyn wirtualnych. Aby przekształcić na oryginalną witrynę podstawowego w aktywnej lokalizacji należy uruchomić planowane trybie awaryjnym. Jeśli pierwotnej witryny nie jest dostępny, mogą nie do jego oryginalnej lokalizacji lub do innej lokalizacji. Aby rozpocząć replikacji ponownie po powrotu do lokalizacji podstawowej rozpoczęciu odwrotnej replikacji.

### <a name="failover-considerations"></a>Zagadnienia dotyczące pracy awaryjnej

- **Adres IP po przełączeniu**— domyślnie nieudanej przez komputer będzie miał inny adres IP niż komputerem źródłowym. Jeśli chcesz zachować samej Zobacz adresu IP: 
    - **Pomocniczy witryny**— Jeśli możesz już awarii pomocniczej witryny i chcesz zachować adres IP, [Przeczytaj](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) ten artykuł. Uwaga można zachować publiczny adres IP Jeśli Usługodawca internetowy obsługuje go.
    - **Azure**— Jeśli możesz już awarii Azure możesz określić adres IP, które chcesz przypisać na karcie **Konfigurowanie** właściwości maszyn wirtualnych. Publiczny adres IP nie może zachować po przełączeniu Azure. Można zachować nie RFC 1918 adres spacje, które są używane jako adresów.
- **Częściowy pracy awaryjnej**, jeśli chcesz zakończyć się niepowodzeniem na części witryny, a nie całą witrynę, należy pamiętać, że: 
    - **Pomocniczy witryny**— Jeśli się nie powieść na części podstawowej witryny do witryny pomocniczej i chcesz się połączyć z powrotem do podstawowej witryny, użyj połączenie VPN witryny do witryny nie powiodło się łączyć za pośrednictwem aplikacji w witrynie pomocniczej elementów infrastruktury uruchomionych podstawowej witryny. Jeśli całą podsieć nie powiedzie się nad można zachować adres IP maszyn wirtualnych. Jeśli nie zostanie przez częściowe podsieci nie może zachować adres IP maszyn wirtualnych, ponieważ podsieci nie można podzielić między witrynami.
    - **Azure**— Jeśli się nie powieść przez częściowe witryny Azure i chcesz się połączyć podstawowej witryny, można użyć sieci VPN witryny do witryny nie powiodło się łączyć za pośrednictwem aplikacji platformy Azure elementów infrastruktury uruchomionych podstawowej witryny. Należy zauważyć, że jeśli nie udaje się całą podsieć przez Ciebie można zachować adres IP maszyn wirtualnych. Jeśli nie zostanie przez częściowe podsieci nie może zachować adres IP maszyn wirtualnych, ponieważ podsieci nie można podzielić między witrynami.
 
- **Litera**, jeśli chcesz zachować literę w środowisku maszyn wirtualnych systemu po przełączeniu można ustawić zasady SAN maszyny wirtualnej **na**. Maszyn wirtualnych dysków trybu online automatycznie. [Dowiedz się więcej](https://technet.microsoft.com/library/gg252636.aspx).
- **Trasa żądań**— Odzyskiwanie witryny współpracy z menedżerem ruch Azure rozsyłanie żądań klienta do aplikacji po przełączeniu.




## <a name="run-a-test-failover"></a>Uruchom w trybie awaryjnym test

Po uruchomieniu trybie awaryjnym test zostanie wyświetlony monit wybierania ustawień sieci na komputerach replice test. Masz wiele opcji.  

**Opcja pracy awaryjnej test** | **Opis** | **Sprawdź pracy awaryjnej** | **Szczegóły**
---|---|---|---
**Niepowodzenie nad Azure — bez sieci** | Nie zaznaczaj docelowej sieci Azure | Testy pracy awaryjnej testowanie maszyn wirtualnych uruchamiany zgodnie z oczekiwaniami platformy Azure | Wszystkie maszyn wirtualnych test w planie odzyskiwanie są dodawane w usłudze w chmurze pojedynczej i można połączyć ze sobą<br/><br/>Komputery nie masz połączenia z siecią Azure po przełączeniu.<br/><br/>Użytkownicy mogą połączyć się z komputerami test z publiczny adres IP
**Niepowodzenie nad Azure — z siecią** | Wybierz element docelowy sieci Azure | Pracy awaryjnej sprawdza, czy komputerów testowych są podłączone do sieci | Utwórz sieć Azure, które samodzielnie z sieci produkcyjnej Azure i konfigurowanie infrastruktury zreplikowanej maszyny wirtualnej działa zgodnie z oczekiwaniami.<br/><br/>Podsieć maszyny wirtualnej testowych jest oparty na podsieci, na którym nie powiodło się nad maszyn wirtualnych oczekuje się, aby nawiązać połączenie jeśli awaria planowanego lub niezaplanowane wystąpiła.
**Nie na pomocniczym witryny VMM — bez sieci** | Nie zaznaczaj maszyn wirtualnych sieci | Pracy awaryjnej sprawdza utworzony komputerów testowych.<br/><br/>Test maszyny wirtualnej zostanie utworzony na tym samym hoście jako host, na którym istnieje maszyny wirtualnej replice. Nie jest dodawany w chmurze, w którym znajduje się maszyny wirtualnej replice. | <p>Nie powiodło się na komputerze nie będzie podłączony do każdej sieci.<br/><br/>Komputer można łączyć się z siecią maszyn wirtualnych po jego utworzeniu
**Nie na pomocniczym witryny VMM — z siecią** | Wybierz istniejącą sieć maszyn wirtualnych | Pracy awaryjnej sprawdza, czy są tworzone maszyn wirtualnych | Test maszyny wirtualnej zostanie utworzony na tym samym hoście jako host, na którym istnieje maszyny wirtualnej replice. Nie jest dodawany w chmurze, w którym znajduje się maszyny wirtualnej replice.<br/><br/>Tworzenie maszyn wirtualnych sieci, w której ma się odizolowane z sieci produkcyjnej<br/><br/>Jeśli korzystasz z sieci VLAN zalecamy tworzenie oddzielnej sieci logiczne (nie używana w produkcji) w VMM w tym celu. Tej sieci logiczne służy do tworzenia maszyn wirtualnych sieci do celów testowych pracy awaryjnej.<br/><br/>Powinny być skojarzone z co najmniej jedną z kart sieciowych z serwerami funkcji Hyper-V hostingu maszyn wirtualnych sieci logicznej.<br/><br/>Sieciach VLAN logiczne witryn sieci dodawany do sieci logicznej powinny być odizolowane.<br/><br/>Jeśli używasz logiczna sieć oparte na wirtualizacji sieci Windows Azure Odzyskiwanie witryny automatycznie tworzy odizolowanych maszyn wirtualnych sieci.
**Nie na pomocniczym witryny VMM — tworzenie sieci** | Sieci tymczasowe testowego będą tworzone automatycznie zgodnie z ustawieniem zadanej w **Sieci logicznej** i jego witrynami sieci | Pracy awaryjnej sprawdza, czy są tworzone maszyn wirtualnych | Użyj tej opcji, jeśli plan odzyskiwania korzysta z więcej niż jedną siecią maszyn wirtualnych. Jeśli korzystasz z sieci Windows sieci wirtualizacji, tę opcję, można automatycznie utworzyć maszyn wirtualnych sieci z tymi samymi ustawieniami (podsieci i pule adresów IP) w replice maszyny wirtualnej sieci. Tych maszyn wirtualnych sieci są automatycznie czyszczone po zakończeniu tym przełączeniu test.</p><p>Test maszyny wirtualnej zostanie utworzony na tym samym hoście jako host, na którym istnieje maszyny wirtualnej replice. Nie jest dodawany w chmurze, w którym znajduje się maszyny wirtualnej replice.

>[AZURE.NOTE] Sam, jak adres IP, otrzyma, kiedy adres IP podane maszyn wirtualnych podczas pracy awaryjnej test robi trybie awaryjnym planowanego lub niezaplanowane (przy założeniu, że adres IP jest dostępna w sieci pracy awaryjnej testowego. Jeśli ten sam adres IP nie jest dostępna w sieci pracy awaryjnej testowego maszyn wirtualnych otrzyma w sieci pracy awaryjnej testowego innego adresu IP.



### <a name="run-a-test-failover-from-on-premises-to-azure"></a>Uruchamianie trybie awaryjnym test z lokalnego Azure

Ta procedura opisano, jak przeprowadzić trybie awaryjnym test dla planu odzyskiwania. Można też kliknąć awaryjnego jednym komputerze można uruchamiać na karcie **maszyn wirtualnych** .

1. Wybierz pozycję **Plany odzyskiwania** > *recoveryplan_name*. Kliknij pozycję **pracy awaryjnej** > **pracy awaryjnej Test**.
2. Na stronie **Potwierdzanie pracy awaryjnej Test** Określ, jak maszyn replice będzie połączony z siecią Azure po przełączeniu.
3. Jeśli możesz już awarii Azure i szyfrowanie danych jest włączone dla chmury, klucza **Szyfrowania** , wybierz certyfikat, który został wydany po włączeniu szyfrowania danych podczas instalacji dostawcy. 
4. Śledzenie postępu pracy awaryjnej na karcie **zadania** . Powinny być widoczne na komputerze replice test w portalu Azure.
5. Można korzystać z komputerów replice platformy Azure z witryny lokalnej inicjowanie połączenie RDP maszyn wirtualnych. port 3389 będzie muszą być otwarte na punkt końcowy dla maszyny wirtualnej.
5. Po wykonaniu, po tym przełączeniu osiągnie fazy **badania wykonane** , kliknij pozycję **Pełne badanie** , aby zakończyć.
5. W **notatkach** Nagraj i zapisz wszelkie uwagi skojarzone z tym przełączeniu test.
8. Kliknij opcję **zakończeniu tym przełączeniu test** Oczyszczanie środowisku testowym. Po ukończeniu tym przełączeniu test będzie stan C**omplete** .

> [AZURE.NOTE] Test trybie awaryjnym składa się z więcej niż dwa tygodnie będzie wypełniane przez siłę. Wszystkie elementy lub maszyn wirtualnych tworzone automatycznie podczas tym przełączeniu test zostaną usunięte.
  

### <a name="run-a-test-failover-from-a-primary-on-premises-site-to-a-secondary-on-premises-site"></a>Uruchamianie trybie awaryjnym test z witryny podstawowy lokalnego do witryny lokalnej pomocniczej

Musisz zrobić na kilka sposobów uruchamiania w trybie awaryjnym testowych, takich jak tworzenie kopii kontrolera domeny i umieszczenie serwerów DNS i DHCP testowych w środowisku testowym. Możesz wykonać w na kilka sposobów:

- Jeśli chcesz uruchomić w trybie awaryjnym test przy użyciu istniejącej sieci, przygotować usługi Active Directory, DHCP i DNS w sieci.
- Jeśli chcesz uruchomić w trybie awaryjnym test za pomocą opcji, aby automatycznie utworzyć maszyn wirtualnych sieci, Dodaj ręcznego krok przed grupy 1 w planie odzyskiwania, których chcesz użyć do przełączania awaryjnego test, a następnie dodać zasoby infrastruktury do sieci tworzonych automatycznie przed uruchomieniem tym przełączeniu test.

#### <a name="things-to-note"></a>Uwag

- Gdy replikacji do witryny pomocniczym, typ sieci używany przez komputer replice nie należy pasujące do typu sieci logicznej używane do przełączania awaryjnego test, ale niektóre kombinacje może nie działać. Jeśli replice korzysta z DHCP i izolacji opartego na VLAN, sieci maszyn wirtualnych replice nie wymaga statycznej puli adresów IP. Za pomocą wirtualizacji sieci systemu Windows do przełączania awaryjnego test nie działa, ponieważ nie pule adresów są dostępne. Ponadto pracy awaryjnej testu nie będzie działać, jeśli sieć replice jest brak izolacji i sieć test jest wirtualizacji sieci systemu Windows. Jest to spowodowane sieć bez izolacji nie ma podsieci wymagane do utworzenia sieci wirtualizacji sieci systemu Windows.
- Sposób, w których replice maszyn wirtualnych podłączonych do zamapować maszyn wirtualnych sieci po pracy awaryjnej zależy od sposobu skonfigurowania sieci maszyn wirtualnych w konsoli VMM:
    - **Sieć maszyn wirtualnych skonfigurowaną bez izolacji i izolacji VLAN**— Jeśli DHCP jest definiowana maszyn wirtualnych sieci, maszyny wirtualnej replice będzie połączony z Identyfikatora sieci przy użyciu określonych ustawień witryny sieci w skojarzonej sieci logiczne. Maszyny wirtualnej pobiera adres IP z dostępnych serwerze. Nie musisz statycznej puli adresów IP zdefiniowanych dla maszyn wirtualnych sieci docelowej. Użycie statycznej puli adresów IP dla sieci maszyn wirtualnych maszyny wirtualnej replice zostanie połączony z Identyfikatora sieci przy użyciu określonych ustawień witryny sieci w skojarzonej sieci logiczne. Maszyny wirtualnej otrzyma adres IP z puli zdefiniowane dla maszyn wirtualnych sieci. Jeśli statyczna pula adresów IP nie jest zdefiniowana w sieci maszyn wirtualnych docelowej, przydzielania adresów IP nie powiedzie się. Pula adresów IP powinny być tworzone na serwerach VMM źródłowej i docelowej, które ma być używana do obsługi ochrony i odzyskiwania.
    - **Sieć maszyn wirtualnych z systemem Windows sieci wirtualizacji**— Jeśli maszyn wirtualnych sieci skonfigurowano ustawienie statyczną pulę należy zdefiniować maszyn wirtualnych sieci docelowej, niezależnie od tego, czy sieć maszyn wirtualnych źródłowa jest skonfigurowany do za pomocą DHCP lub statyczny adres IP puli adresów. Po zdefiniowaniu DHCP na docelowym serwerze VMM działają jako ten serwer i podać adres IP z puli, która jest zdefiniowana maszyn wirtualnych sieci docelowej. Jeśli zdefiniowano stosowania statycznej puli adresów IP serwera źródłowego, na docelowym serwerze VMM będą przydzielane adres IP z puli. W obu przypadkach przydzielania adresów IP zakończy się niepowodzeniem, jeśli nie zdefiniowano statycznej puli adresów IP.

#### <a name="run-test"></a>Uruchom test

Ta procedura opisano, jak przeprowadzić trybie awaryjnym test dla planu odzyskiwania. Alternatywnie można uruchamiać awaryjnego pojedynczy maszyn wirtualnych lub serwera fizycznego na karcie **maszyn wirtualnych** .

1. Wybierz pozycję **Plany odzyskiwania** > *recoveryplan_name*. Kliknij pozycję **pracy awaryjnej** > **pracy awaryjnej Test**.
2. Na stronie **Potwierdzanie Testowanie pracy awaryjnej** Określ, jak maszyn wirtualnych powinien być połączony z sieci po przełączeniu test.
3. Śledzenie postępu pracy awaryjnej na karcie **zadania** . Po tym przełączeniu osiągnie fazy** badania wykonane** , kliknij przycisk **Testowanie wykonania** , aby zakończyć tym przełączeniu test.
4. Kliknij **Notes** , aby nagrać, a wszelkie uwagi skojarzone z tym przełączeniu test.
4. Po zakończeniu sprawdź pomyślne uruchomienie maszyn wirtualnych.
5. Po zweryfikowaniu pomyślne uruchomienie maszyn wirtualnych, wykonaj przełączanie awaryjne testowych do Oczyść odizolowanych środowiska. Jeśli zdecydujesz się automatycznie utworzyć maszyn wirtualnych sieci, oczyszczanie powoduje usunięcie wszystkich maszyn wirtualnych test i testowanie sieci.

> [AZURE.NOTE] Test trybie awaryjnym składa się z więcej niż dwa tygodnie będzie wypełniane przez siłę. Wszystkie elementy lub maszyn wirtualnych tworzone automatycznie podczas tym przełączeniu test zostaną usunięte.


#### <a name="prepare-dhcp"></a>Przygotowywanie DHCP

Jeśli w środowisku maszyn wirtualnych systemu pracy awaryjnej test za pomocą DHCP, serwerze test ma zostać utworzona w ramach sieci odizolowanych utworzony do celów testowych pracy awaryjnej.


### <a name="prepare-active-directory"></a>Przygotowywanie usługi Active Directory
Aby uruchomić test awaryjnego testowania aplikacji, musisz kopię w środowisku produkcyjnym usługi Active Directory w środowisku testowym. Przejdź do sekcji [Testowanie pracy awaryjnej zagadnienia związane z usługą active directory](site-recovery-active-directory.md#considerations-for-test-failover) , aby uzyskać więcej informacji. 


### <a name="prepare-dns"></a>Przygotowywanie DNS

Przygotowywanie serwera DNS do przełączania awaryjnego test w następujący sposób:

- **DHCP**— maszyn wirtualnych użycie DHCP aktualizacji adres IP testu DNS na serwerze test. Jeśli używasz typu sieci wirtualizacji sieci Windows VMM server działa jako serwerze. W związku z tym adres IP serwera DNS powinny być aktualizowane w sieci pracy awaryjnej testowego. W tym przypadku maszyn wirtualnych będzie rejestrować do odpowiedniego serwera DNS.
- **Statyczny adres**— Jeśli maszyn wirtualnych za pomocą statycznego adresu IP, adres IP serwera DNS test należy zostaną zaktualizowane w sieci pracy awaryjnej testowego. Może być konieczne zaktualizowanie DNS z adresem IP maszyn wirtualnych testowych. Skorzystaj z tego skryptu próbki, w tym celu: 

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-planned-failover-primary-to-secondary"></a>Uruchamianie zaplanowanych trybie awaryjnym (podstawowy do pomocniczego)

 Ta procedura opisano sposób uruchamiania zaplanowanych trybie awaryjnym planu odzyskiwania. Można też kliknąć można uruchamiać awaryjnego jednym komputerze wirtualnych na karcie **maszyn wirtualnych** .

1. Przed rozpoczęciem pracy upewnij się, że wszystkie maszyn wirtualnych chcesz zakończyć się niepowodzeniem na ukończenie replikacji początkowej.
2. Wybierz pozycję **Plany odzyskiwania** > *recoveryplan_name*. Kliknij pozycję **pracy awaryjnej** > **Planowana pracy awaryjnej**. 
3. Na stronie **Potwierdzanie pracy awaryjnej planowana **wybierz lokalizacji źródłowej i docelowej. Uwaga kierunek pracy awaryjnej.

    - Jeśli wszystkie serwery maszyn wirtualnych znajdują się w lokalizacji źródłowej lub docelowej z poprzedniego praca awaryjna działały zgodnie z oczekiwaniami, szczegóły kierunek pracy awaryjnej są jedynie w celach informacyjnych. 
    - Jeśli maszyn wirtualnych są aktywne w lokalizacji źródłowej i docelowej, wyświetlany jest przycisk **Zmień kierunek** . Ten przycisk służy do zmiany i określić kierunek, w którym tym przełączeniu powinna zostać wykonana.

5. Jeśli możesz już awarii Azure i szyfrowanie danych jest włączone dla chmury, klucza **Szyfrowania** , wybierz certyfikat, który został wydany po włączeniu szyfrowania danych podczas instalacji dostawcy na serwerze VMM. 
6. Po rozpoczęciu planowane trybie awaryjnym pierwszym krokiem jest zamknięty maszyn wirtualnych, aby zapobiec utracie danych. Na karcie **zadania** można śledzić postęp pracy awaryjnej. Jeśli wystąpi błąd, w tym przełączeniu (lub na komputerze wirtualnych w obszarze skrypt, który znajduje się w planie odzyskiwania), przestaje planowane przejęcie awaryjne planu odzyskiwania. Można ponownie zainicjować tym przełączeniu.
8. Po utworzeniu maszyn wirtualnych replice są one Zatwierdź stanie oczekiwania. Kliknij przycisk **Zatwierdź** , aby zatwierdzić tym przełączeniu. 
9. Po replikacji wykonywanie uruchamiania maszyn wirtualnych w lokalizacji pomocniczej. 

## <a name="run-an-unplanned-failover"></a>Uruchamianie nieplanowanego przełączania awaryjnego

Tej procedurze opisano sposób uruchamiania nieplanowanego przełączania awaryjnego dla planu odzyskiwania. Alternatywnie można uruchamiać awaryjnego pojedynczy maszyn wirtualnych lub serwera fizycznego na karcie **maszyn wirtualnych** .

1. Wybierz pozycję **Plany odzyskiwania** > *recoveryplan_name*. Kliknij pozycję **pracy awaryjnej** > **nieplanowanego przełączania awaryjnego**. 
3. Na stronie **Potwierdzanie nieplanowanego przełączania awaryjnego **wybierz lokalizacji źródłowej i docelowej. Uwaga kierunek pracy awaryjnej.

    - Jeśli wszystkie serwery maszyn wirtualnych znajdują się w lokalizacji źródłowej lub docelowej z poprzedniego praca awaryjna działały zgodnie z oczekiwaniami, szczegóły kierunek pracy awaryjnej są jedynie w celach informacyjnych. 
    - Jeśli maszyn wirtualnych są aktywne w lokalizacji źródłowej i docelowej, wyświetlany jest przycisk **Zmień kierunek** . Ten przycisk służy do zmiany i określić kierunek, w którym tym przełączeniu powinna zostać wykonana.

4. Jeśli możesz już awarii Azure i szyfrowanie danych jest włączone dla chmury, klucza **Szyfrowania** , wybierz certyfikat, który został wydany po włączeniu szyfrowania danych podczas instalacji dostawcy na serwerze VMM. 
5. Wybierz **zamknięty maszyn wirtualnych i synchronizowanie najnowszych danych** Określ, czy Odzyskiwanie witryny należy spróbować zamknąć chronionego maszyn wirtualnych i zsynchronizować dane, tak aby najnowszą wersję pakietu danych nie powiedzie się nad. Jeśli nie zaznaczysz tej opcji lub próba nie powiodła się tym przełączeniu będą od punktu najnowszą odzyskiwania dostępne dla maszyny wirtualnej.
6. Na karcie **zadania** można śledzić postęp pracy awaryjnej. Należy zauważyć, że nawet w przypadku wystąpienia błędów podczas nieplanowanego przełączania awaryjnego, planu odzyskiwania zostanie uruchomiona aż do zakończenia.
7. Po przełączeniu maszyn wirtualnych są w stanie **zatwierdzić oczekujące** . Kliknij przycisk **Zatwierdź** , aby zatwierdzić tym przełączeniu.
8. Jeśli skonfigurujesz replikacji używać wiele punktów odzyskiwania w punkt odzyskiwania zmień możesz wybrać punkt odzyskiwania, którego nie ma najnowsze (r jest używany domyślnie). Po zatwierdzeniu dodatkowe punkty odzyskiwania zostaną usunięte.
9. Po zakończeniu replikacji maszyn wirtualnych uruchamiania i są uruchomione w lokalizacji pomocniczej. Jednak nie są chronione, czy replikujących. Po podstawowej witryny jest dostępna ponownie z tym samym infrastruktury podstawowej, kliknij przycisk **Odwrotnej powielić** zacząć odwrotnej replikacji. Dzięki temu, że wszystkie dane są replikowane powrót do podstawowej witryny, a maszyny wirtualnej jest gotowa do przełączania awaryjnego ponownie. Odwracanie replikacji po nieplanowanego przełączania awaryjnego wiąże się obciążenie transferu danych. Przeniesienie będzie używać tej samej metody, który skonfigurowano ustawień replikacji początkowej chmury.

## <a name="failback-from-secondary-to-primary"></a>Awarii z pomocniczym podstawowy

 Po przełączeniu z podstawowej do lokalizacji pomocniczej zreplikowanej maszyn wirtualnych nie są chronione przez Odzyskiwanie witryny, a pomocniczym lokalizacji obecnie działa jako podstawowy. Wykonaj poniższe procedury nie na oryginalną witrynę podstawowego. Ta procedura opisano sposób uruchamiania zaplanowanych trybie awaryjnym planu odzyskiwania. Można też kliknąć można uruchamiać awaryjnego jednym komputerze wirtualnych na karcie **maszyn wirtualnych** .

1. Wybierz pozycję **Plany odzyskiwania** > *recoveryplan_name*. Kliknij pozycję **pracy awaryjnej** > **Planowana pracy awaryjnej**.
2. Na stronie **Potwierdzanie pracy awaryjnej planowana **wybierz lokalizacji źródłowej i docelowej. Uwaga kierunek pracy awaryjnej. Jeśli awaryjne z podstawowego pracy jako się spodziewać i wszystkich maszyn wirtualnych znajdują się w lokalizacji pomocniczej, który jest jedynie w celach informacyjnych.
3. Jeśli w przypadku braku ponownie z platformy Azure wybierz ustawienia **Synchronizacji danych**:

    - **Synchronizowanie danych przed pracy awaryjnej (tylko zmiany różnicy Synchronizuj)**— ta opcja minimalizuje przestoje maszyn wirtualnych, jak będą synchronizowane bez ich zamykania. Funkcja wykonuje następujące czynności:
        - Faza 1: Pobiera migawkę maszyny wirtualnej Azure i skopiowanie go do lokalnego hosta funkcji Hyper-V. Komputer nadal działa w Azure.
        - Faza 2: Zamyka maszyny wirtualnej Azure, aby było braku nowych zmian. Ostateczny zestaw zmiany są przenoszone do lokalnego serwera i uruchomieniem maszyny wirtualnej lokalnego.
    

    - **Synchronizowanie danych podczas pracy awaryjnej tylko (pełne pobieranie)**— Użyj tej opcji, jeśli została zostało uruchomione Azure przez dłuższy czas. Ta opcja jest szybciej, ponieważ zmienił się najczęściej dysku, a firma Microsoft nie chcesz poświęcać czasu obliczania sumy kontrolnej. Wykonuje pobierania dysku. Jest również przydatne, gdy lokalna maszyny wirtualnej zostały usunięte.
    
    > [AZURE.NOTE] Zalecamy, użyj tej opcji, jeśli została Azure zostało uruchomione przez pewien czas (miesiąca lub więcej) lub maszyn wirtualnych lokalna został usunięty. Ta opcja nie wykonywać obliczenia sumy kontrolnej.
    
5. Jeśli możesz już awarii Azure i szyfrowanie danych jest włączone dla chmury, klucza **Szyfrowania** , wybierz certyfikat, który został wydany po włączeniu szyfrowania danych podczas instalacji dostawcy na serwerze VMM. 
5. Domyślnie jest używana ostatni punkt odzyskiwania, ale **Zmień punkt odzyskiwania** można określić punkt różnych odzyskiwania. 
6. Kliknij znacznik wyboru, aby rozpocząć awarii.  Na karcie **zadania** można śledzić postęp pracy awaryjnej. 
7. f wybrano opcję Synchronizuj danych przed tym przełączeniu po zakończeniu synchronizacji danych początkowych i będzie gotowe zamknąć maszyn wirtualnych w Azure, kliknij pozycję **zadania**  >  <planned failover job name> **Wykonania pracy awaryjnej**. Zamknięcie komputera Azure, przenosi najnowszych zmian maszyn wirtualnych lokalnego i uruchamia go.
8. Teraz możesz zalogowaniu maszyny wirtualnej, aby sprawdzić poprawność jej jest dostępna, zgodnie z oczekiwaniami. 
9. Wirtualna maszyna jest w Zatwierdź stanie oczekiwania. Kliknij przycisk **Zatwierdź** , aby zatwierdzić tym przełączeniu.
10. Teraz w celu ukończenia awarii kliknij opcję **Odwróć powielić** zacząć ochrona maszyny wirtualnej w podstawowej witryny.



## <a name="failback-to-an-alternate-location"></a>Powrotu do innej lokalizacji

Jeśli został wdrożony ochrony między [witryny funkcji Hyper-V i Azure](site-recovery-hyper-v-site-to-azure.md) należy możliwość powrotu Azure, aby alternatywny lokalnych lokalizacji. Jest to przydatne, jeśli musisz skonfigurować nowy sprzęt lokalnego. Poniżej pokazano, jak to zrobić.

1. Jeśli konfigurujesz nowego sprzętu Zainstaluj Windows Server 2012 R2 i roli Hyper-V na serwerze.
2. Tworzenie przełącznika wirtualną sieć o takiej samej nazwie mająca na oryginalnym serwerze.
3. Wybierz **Elementy chronione** -> **Grupa ochrona**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> chcesz powrócić i wybierz pozycję **Planowana pracy awaryjnej**.
4. W **Potwierdzić pracy awaryjnej planowana** wybierz **Tworzenie maszyn wirtualnych lokalnego Jeśli nie istnieje**. 
5. W polu **Nazwa hosta** wybierz nowy serwer hosta funkcji Hyper-V, w którym chcesz umieścić maszyny wirtualnej.
6. W synchronizacji danych zaleca się, że zaznaczono opcję **Synchronizuj danych przed tym przełączeniu**. Minimalizuje przestoje maszyn wirtualnych, jak będą synchronizowane bez ich zamykania. Funkcja wykonuje następujące czynności:

    - Faza 1: Pobiera migawkę maszyny wirtualnej Azure i skopiowanie go do lokalnego hosta funkcji Hyper-V. Komputer nadal działa w Azure.
    - Faza 2: Zamyka maszyny wirtualnej Azure, aby było braku nowych zmian. Ostateczny zestaw zmiany są przenoszone do lokalnego serwera i uruchomieniem maszyny wirtualnej lokalnego.
    
7. Kliknij znacznik wyboru, aby rozpocząć tym przełączeniu (awarii).
8. Po zakończeniu początkowej synchronizacji i będzie już gotowe do zamknięcia maszyny wirtualnej Azure, kliknij pozycję **zadania** > <planned failover job> > **Wykonania pracy awaryjnej**. Zamknięcie Azure komputera, przenosi najnowszych zmian w lokalnym maszyn wirtualnych i uruchamia go.
9. Możesz zalogować się na komputerze wirtualnych lokalnego, aby sprawdzić, czy wszystko działa zgodnie z oczekiwaniami. Następnie kliknij przycisk **Zatwierdź** , aby zakończyć tym przełączeniu.
10. Kliknij opcję **Odwróć powielić** zacząć ochrona maszyny wirtualnej lokalnego.

    >[AZURE.NOTE] Jeśli możesz anulować zadanie awarii, gdy będzie ona w kroku synchronizacji danych, maszyn wirtualnych w lokalnym będzie w stanie uszkodzone. Jest to spowodowane wykonuje synchronizacja danych kopie najnowszych danych ze Azure maszyn wirtualnych dysków dyski danych lokalna, untill synchronizacji, dysk, na którym dane mogą nie być spójna. Jeśli maszyn wirtualnych na prem uruchomieniu po anulowaniu synchronizacja danych może nie uruchomić. Ponownie wyzwalanie przełączanie awaryjne do wykonania synchronizacja danych.
 
