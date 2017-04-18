<properties
    pageTitle="Chronienie usługi Active Directory i DNS z odzyskiwanie Azure witryny | Microsoft Azure"
    description="Ten artykuł zawiera opis jak wdrażać rozwiązania odzyskiwania po awarii usługi Active Directory przy użyciu Odzyskiwanie witryny Azure."
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="08/31/2016"
    ms.author="pratshar"/>

# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Chronienie usługi Active Directory i DNS z odzyskiwanie Azure witryny

Aplikacji przedsiębiorstwa, takich jak SharePoint, Dynamics AX i rozwiązania SAP zależą od usługi Active Directory i infrastruktury DNS działać prawidłowo. Możesz utworzyć rozwiązanie odzyskiwania danych dla aplikacji, należy należy pamiętać, że należy ochrony i odzyskiwania usługi Active Directory i systemie DNS przed inne składniki aplikacji, aby upewnić się, że czynności działać poprawnie po wystąpieniu awarii.

Odzyskiwanie witryny to usługa Azure, która zapewnia awarii przy orchestrating replikacji, pracy awaryjnej i odzyskiwanie maszyn wirtualnych. Odzyskiwanie witryny obsługuje wiele scenariuszy replikacji spójne ochrony, bezproblemowo maszyn wirtualnych pracy awaryjnej i aplikacji do chmury prywatna, publiczna lub hosta.

Za pomocą Odzyskiwanie witryny, możesz utworzyć planu odzyskiwania pełną automatyczną danych usługi Active Directory. W przypadku wystąpienia zakłóceń można zainicjować trybie awaryjnym w ciągu kilku sekund z dowolnego miejsca i wdrażanie i uruchamianie usługi Active Directory w ciągu kilku minut. Jeśli został wdrożony usługi Active Directory dla wielu aplikacji, takich jak SharePoint i rozwiązania SAP w witrynie podstawowego i chcesz się nie powieść nad pełną witryną, użytkownik może się nie powieść w usłudze Active Directory najpierw Odzyskiwanie witryny, a następnie kończą się niepowodzeniem w innych aplikacjach przy użyciu planów odzyskiwania specyficzne dla aplikacji.

W tym artykule wyjaśniono, jak utworzyć rozwiązanie odzyskiwania po awarii usługi Active Directory, jak przeprowadzić planowanego, niezaplanowane i praca awaryjna test przy użyciu planu odzyskiwania jednym kliknięciem, obsługiwanych konfiguracji i wymagania wstępne.  Należy zapoznać się z usługą Active Directory i odzyskiwanie witryny Azure, przed rozpoczęciem pracy.

Dostępne są dwie opcje zalecane, zależnie od stopnia złożoności środowiska.

### <a name="option-1"></a>Opcja 1

Jeśli masz małą liczbę programy, a następnie pojedynczy kontroler domeny i chcesz zakończyć się niepowodzeniem w całej witryny, następnie zalecamy używanie Odzyskiwanie witryny replikacji kontrolera domeny do witryny pomocniczą (tego, czy możesz już awarii nad Azure lub do witryny pomocniczej). Tej samej zreplikowanej maszyny wirtualnej można używać do przełączania awaryjnego test zbyt.

### <a name="option-2"></a>Opcja 2

Jeśli masz dużą liczbą aplikacji, a istnieje więcej niż jeden kontroler domeny w środowisku lub jeśli planujesz niepowodzenie przez kilka aplikacji naraz, zalecamy, oprócz replikacji maszyny wirtualnej kontrolera domeny z Odzyskiwanie witryny będą również skonfigurować dodatkowego kontrolera domeny w witrynie docelowej (Azure lub centrum danych lokalnych pomocniczej).

>[AZURE.NOTE] Nawet jeśli opcja-2, w przypadku wykonywania do wykonując trybie awaryjnym testowych nadal musisz powielić kontrolera domeny przy użyciu Odzyskiwanie witryny. W tym artykule [Testowanie pracy awaryjnej zagadnienia](#considerations-for-test-failover) , aby uzyskać więcej informacji.


W następujących sekcjach wyjaśniono, jak włączyć ochronę kontrolera domeny w witrynie odzyskiwania oraz sposobu konfigurowania kontrolera domeny w Azure.


## <a name="prerequisites"></a>Wymagania wstępne

- Wdrożenia lokalnego serwera DNS i usługi Active Directory.
- Magazynu usługi odzyskiwania witryny Azure w subskrypcji Microsoft Azure.
- Jeśli instalacja Azure już replikacji Narzędzie Azure Ocena gotowości maszyn wirtualnych na maszyny wirtualne, aby upewnić się, są one zgodne z maszyny wirtualne Azure i usługi odzyskiwania witryny Azure.


## <a name="enable-protection-using-site-recovery"></a>Włącz ochronę przy użyciu Odzyskiwanie witryny


### <a name="protect-the-virtual-machine"></a>Ochrona maszyny wirtualnej

Włącz ochronę maszyny wirtualnej DNS kontrolera domeny w witrynie odzyskiwania. Konfigurowanie ustawień Odzyskiwanie witryny na podstawie typu maszyn wirtualnych (funkcji Hyper-V lub VMware). Zalecamy częstotliwości replikacji spójne awarie 15 minut.

###<a name="configure-virtual-machine-network-settings"></a>Konfigurowanie ustawień sieci maszyn wirtualnych

Maszyny wirtualnej DNS kontrolera domeny skonfigurować ustawienia sieci w Odzyskiwanie witryny, tak aby maszyn wirtualnych zostaną dołączone do sieci prawo po przełączeniu. Na przykład jeśli masz replikacji maszyny wirtualne funkcji Hyper-V Azure można wybrać maszyn wirtualnych w chmurze VMM lub w grupie ochrona w celu skonfigurowania ustawień sieci, tak jak pokazano poniżej

![Ustawienia sieci maszyn wirtualnych](./media/site-recovery-active-directory/VM-Network-Settings.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Ochrony usługi Active Directory z replikacją usługi Active Directory

### <a name="site-to-site-protection"></a>Ochrona witryny do witryny

Tworzenie kontrolera domeny w witrynie pomocniczej i określić nazwę tej samej domeny używanej w witrynie podstawowego podczas poziom jest podwyższany do roli kontrolera domeny. Za pomocą przystawki **Lokacje i Active Directory usług** do konfigurowania ustawień na obiekt łącza witryny, do której zostaną dodane witryn. Konfigurując ustawienia łącze witryny, można kontrolować, gdy wystąpi replikacji między dwa lub więcej witryn i jak często. Aby uzyskać więcej informacji, zobacz [Planowanie replikacji między witrynami](https://technet.microsoft.com/library/cc731862.aspx) .

###<a name="site-to-azure-protection"></a>Ochrona witryny do Azure

Postępuj zgodnie z instrukcjami, aby [utworzyć kontroler domeny w Azure wirtualnej sieci](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Gdy poziom jest podwyższany do roli kontrolera domeny Określ tą samą nazwą domeny używanej w witrynie podstawowego.

Następnie [skonfigurowanie serwera DNS wirtualną sieć](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), aby użyć serwera DNS Azure.

![Sieć Azure](./media/site-recovery-active-directory/azure-network.png)

## <a name="test-failover-considerations"></a>Testowanie pracy awaryjnej zagadnienia

Test awaria wystąpiła w sieci, w której ma samodzielnie z sieci produkcyjnej tak, aby nie ma żadnego wpływu na obciążenia produkcji.

Większość aplikacji również wymaga obecności kontrolera domeny i serwera DNS działał, więc przed niepowodzeniem aplikacji na kontrolerze domeny musi być utworzony w odizolowanych sieć ma być używana do przełączania awaryjnego test. Najprostszym sposobem na tym jest włączyć ochronę komputera wirtualnych kontroler DNS domeny z Odzyskiwanie witryny, a następnie uruchom test trybie awaryjnym tej maszyny wirtualnej przed uruchomieniem trybie awaryjnym test planu odzyskiwania aplikacji. Oto, jak to zrobić:

1. Włącz ochronę w Odzyskiwanie witryny dla maszyny wirtualnej DNS kontrolera domeny.
2. Tworzenie odizolowanych sieci. Wszelkie wirtualnej sieci domyślnie tworzone w Azure jest odizolowane od innych sieciach. Zaleca się, że zakres adresów IP dla tej sieci jest identyczny sieci produkcyjnej. Nie umożliwiają łączności witryny do witryny w tej sieci.
3. Podaj adres IP serwera DNS w sieci utworzony, jak adres IP, której oczekujesz maszyny wirtualnej DNS, aby uzyskać. Jeśli możesz już replikacji Azure, wprowadź adres IP dla maszyn wirtualnych, który będzie używany w pracy awaryjnej w **Docelowej IP** ustawienia właściwości maszyn wirtualnych. Jeśli masz replikacji do innego lokalnej witryny i używasz DHCP postępuj zgodnie z instrukcjami, aby [ustawienia DNS i DHCP do przełączania awaryjnego test](site-recovery-failover.md#prepare-dhcp)

>[AZURE.NOTE] Adres IP przydzielonego maszyn wirtualnych w trybie awaryjnym test jest taki sam jak adres IP, którą go w trybie awaryjnym planowanej lub niezaplanowane, jeśli adres IP jest dostępna w sieci pracy awaryjnej testowych. Jeśli nie, maszyny wirtualnej otrzymuje inny adres IP, który jest dostępny w sieci pracy awaryjnej testowego.

4. Na komputerze wirtualnych kontrolera domeny uruchomić awaryjnego testowych przeniesienia go w sieci odizolowane. W tym przełączeniu test za pomocą najnowszą dostępną aplikację punkt odzyskiwania spójne maszyny wirtualnej kontrolera domeny. 
5. Uruchom trybie awaryjnym test dla planu odzyskiwania aplikacji.
6. Po zakończeniu badania Oznacz zadania testowego pracy awaryjnej maszyn wirtualnych kontrolera domeny i planu odzyskiwania "Wykonano" na karcie **zadania** w portalu Odzyskiwanie witryny.

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS i kontroler domeny na różnych komputerach

Jeśli DNS nie na tym samym komputerze wirtualnych jako kontrolera domeny, musisz utworzyć maszyny DNS do przełączania awaryjnego test. Jeśli znajdują się one na tym samym maszyn wirtualnych, możesz pominąć tę sekcję.

Można użyć nowy serwer DNS i Utwórz wszystkie wymagane strefy. Na przykład jeśli Twoja domena usługi Active Directory jest contoso.com, możesz utworzyć strefy DNS z contoso.com nazwę. Pozycje odpowiadające usługi Active Directory należy zaktualizować w systemie DNS w następujący sposób:

1. Upewnij się, że te ustawienia obowiązują przed innych maszyn wirtualnych w planie odzyskiwania pojawi się:

    - Po nazwie głównego las musi mieć nazwę strefy.
    - Strefy musi być plik kopii.
    - Strefy musi być włączony aktualizacji zabezpieczeń i niebezpieczne.
    - Połączenie z adresem IP maszyny wirtualnej DNS powinny wskazywać rozpoznawania nazw maszyny wirtualnej kontrolera domeny.

2. Na komputerze wirtualnych kontrolera domeny, uruchom następujące polecenie:

    `nltest /dsregdns`

3. Dodawanie strefy na serwerze DNS, Zezwalaj na aktualizacje, które nie są bezpieczne i dodać wpis dla niego DNS:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1


## <a name="next-steps"></a>Następne kroki

Przeczytaj [obciążenia, jakie można chronić?](../site-recovery/site-recovery-workload.md) Aby dowiedzieć się więcej o ochronie obciążenia przedsiębiorstwa przy użyciu Odzyskiwanie witryny Azure.
