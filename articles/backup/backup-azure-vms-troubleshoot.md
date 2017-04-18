<properties
    pageTitle="Rozwiązywanie problemów z kopii zapasowej Azure maszyn wirtualnych | Microsoft Azure"
    description="Rozwiązywanie problemów z kopii zapasowych i przywracanie Azure maszyn wirtualnych"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Rozwiązywanie problemów z kopii zapasowej Azure maszyn wirtualnych

> [AZURE.SELECTOR]
- [Odzyskiwanie usługi magazynu](backup-azure-vms-troubleshoot.md)
- [Magazyn kopii zapasowej](backup-azure-vms-troubleshoot-classic.md)

Możesz rozwiązać problemów napotkanych podczas kopii zapasowej Azure za pomocą informacji wymienione w poniższej tabeli.

## <a name="backup"></a>Wykonywanie kopii zapasowych

| Wykonywanie kopii zapasowej | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
|Wykonywanie kopii zapasowych|    Nie można wykonać operacji, ponieważ istnieje już maszyn wirtualnych. -Zatrzymaj ochronę maszyn wirtualnych bez usuwania danych kopii zapasowej. Więcej informacji na http://go.microsoft.com/fwlink/?LinkId=808124   |Dzieje się tak, gdy podstawowy maszyn wirtualnych zostanie usunięty, ale politykę wykonywania kopii zapasowych w dalszym ciągu poszukaj maszyn wirtualnych do wykonywania kopii zapasowych. Aby naprawić ten błąd: <ol><li> Ponowne tworzenie maszyny wirtualnej z tej samej nazwie i tym samym Nazwa grupy zasobów [nazwa usługi cloud],<br>(LUB)</li><li> Zatrzymaj ochronę maszyn wirtualnych z lub bez usuwania danych kopii zapasowej. [Więcej informacji](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Wykonywanie kopii zapasowych|Nie można komunikować się z agentem maszyn wirtualnych migawkę stanu. -Upewnij się, że maszyn wirtualnych ma dostęp do Internetu. Ponadto zaktualizuj agenta maszyn wirtualnych wymienione w podręczniku rozwiązywania problemów w http://go.microsoft.com/fwlink/?LinkId=800034 |Ten błąd jest generowany, jeśli występuje problem z agentem maszyn wirtualnych lub dostęp do sieci Azure infrastruktury jest zablokowany w inny sposób. Dowiedz się więcej na temat debugowania w górę maszyn wirtualnych migawki problemów.<br> Jeśli agenta maszyn wirtualnych nie powoduje problemy, a następnie ponownie uruchom maszyn wirtualnych. W czasie niepoprawnym stanie maszyn wirtualnych może powodować problemy z i ponowne uruchamianie maszyn wirtualnych zresetowanie ten "złym stanie"|
|Wykonywanie kopii zapasowych|    Rozszerzenie usług odzysk nie powiodło się. — Należy upewnić się, że najnowszą wersję agent maszyn wirtualnych znajduje się na komputerze wirtualnych i działa Usługa agenta. Spróbuj ponownie wykonywanie kopii zapasowej i jeśli nie powiedzie się, skontaktuj się z pomocą techniczną firmy Microsoft.|   Ten błąd jest generowany po agenta maszyn wirtualnych jest nieaktualny. Można znaleźć w sekcji "Aktualizowanie agenta maszyn wirtualnych" poniżej, aby zaktualizować agenta maszyn wirtualnych.|
|Wykonywanie kopii zapasowych |Maszyn wirtualnych nie istnieje. — Podaj upewnij się, że istnieje tego maszyn wirtualnych lub wybierz inny maszyny wirtualnej. | Dzieje się tak, gdy podstawowy maszyn wirtualnych zostanie usunięty, ale politykę wykonywania kopii zapasowych w dalszym ciągu poszukaj maszyn wirtualnych do wykonywania kopii zapasowych. Aby naprawić ten błąd: <ol><li> Ponowne tworzenie maszyny wirtualnej z tej samej nazwie i tym samym Nazwa grupy zasobów [nazwa usługi cloud],<br>(LUB)<br></li><li>Zatrzymaj ochronę maszyn wirtualnych bez usuwania danych kopii zapasowej. [Więcej informacji](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Wykonywanie kopii zapasowych |Wykonanie polecenia nie powiodło się. -Inna operacja jest w toku dla tego elementu. Poczekaj, aż do zakończenia poprzedniej operacji, a następnie ponów próbę |Istniejącą kopię zapasową lub Przywróć zadanie maszyn wirtualnych jest uruchomiony, a nie można uruchomić nowe zadanie, gdy jest uruchomiona istniejące zadanie.|
| Wykonywanie kopii zapasowych | Kopiowanie wirtualnych dysków twardych z kopii zapasowej magazynu przekroczony - ponów operację w ciągu kilku minut. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft. | Dzieje się tak, gdy jest zbyt dużo danych do skopiowania. Sprawdź, czy masz mniej niż 16 dyski danych. |
| Wykonywanie kopii zapasowych | Wykonywanie kopii zapasowych nie powiodło się ze względu na błąd wewnętrzny — ponów operację w ciągu kilku minut. Jeśli problem będzie nadal występował, skontaktuj się z pomocą techniczną firmy Microsoft Support | Ten błąd został wyświetlony 2 powodów: <ol><li> Występuje problem przejściowych w dostępie do magazynowania maszyn wirtualnych. Sprawdź [Stan Azure](https://azure.microsoft.com/en-us/status/) , aby sprawdzić, czy jest w toku kwestie związane z obliczeń i miejsca do magazynowania i sieci w regionie. Sprawdź ponów próbę problem kopii zapasowej wpis jest nieco ograniczane. <li>Oryginalny maszyn wirtualnych została usunięta i nie może przyjąć kopii zapasowej. Aby zachować dane kopii zapasowej dla usuniętych maszyn wirtualnych, ale kopii zapasowej błędy zatrzymania, wyłączyć ochronę maszyn wirtualnych i wybierz opcję, aby zachować dane. Spowoduje to zatrzymanie Harmonogram kopii zapasowej, a także cyklicznego komunikaty o błędach. |
| Wykonywanie kopii zapasowych | Nie można zainstalować usługi odzyskiwania Azure rozszerzenie zaznaczonego elementu - Agent maszyn wirtualnych jest wstępnie wymagane dla rozszerzenia usług Azure odzyskiwania. Zainstaluj agenta maszyn wirtualnych Azure i ponownie uruchomić operację rejestracji | <ol> <li>Sprawdź, jeśli agenta maszyn wirtualnych została zainstalowana poprawnie. <li>Upewnij się, czy flaga konfiguracji maszyn wirtualnych jest ustawiona prawidłowo.</ol> [Dowiedz się więcej](#validating-vm-agent-installation) o instalacji agenta maszyn wirtualnych i sprawdzania poprawności instalacji agenta maszyn wirtualnych. |
| Wykonywanie kopii zapasowych | Rozszerzenie Instalacja nie powiodła się z powodu błędu "COM + nie może skontaktować się z koordynatorem transakcji Microsoft Distributed | Zazwyczaj oznacza to, że Usługa COM + nie działa. Kontakt z pomocą techniczną firmy Microsoft, aby uzyskać pomoc dotyczącą rozwiązywania tego problemu. |
| Wykonywanie kopii zapasowych | Migawka niepowodzenie operacji VSS operacji "ten dysk jest zablokowany przez szyfrowanie dysków funkcją BitLocker. Musisz odblokować ten dysk z poziomu Panelu sterowania. | Wyłącz funkcję BitLocker dla wszystkich dysków na maszyn wirtualnych i obserwować, jeśli VSS problemu |
| Wykonywanie kopii zapasowych | Maszyn wirtualnych o wirtualnych dyskach twardych przechowywane w magazynie Premium nie są obsługiwane przez wykonywanie kopii zapasowych | Brak |
| Wykonywanie kopii zapasowych | Nie można odnaleźć Azure maszyn wirtualnych. | Dzieje się tak, gdy podstawowy maszyn wirtualnych zostanie usunięty, ale politykę wykonywania kopii zapasowych w dalszym ciągu poszukaj maszyn wirtualnych do wykonywania kopii zapasowych. Aby naprawić ten błąd: <ol><li>Ponowne tworzenie maszyny wirtualnej z tej samej nazwie i tym samym Nazwa grupy zasobów [nazwa usługi cloud], <br>(LUB) <li> Wyłączyć ochrony dla tego maszyn wirtualnych, dzięki czemu nie zostaną utworzone zadania kopii zapasowej </ol> |
| Wykonywanie kopii zapasowych | Agent maszyn wirtualnych nie znajduje się na komputerze wirtualnych - Zainstaluj wymaganego wymagania wstępne agenta maszyn wirtualnych, a następnie ponownie uruchomić operację. | [Dowiedz się więcej](#vm-agent) o instalacji agenta maszyn wirtualnych i sprawdzania poprawności instalacji agenta maszyn wirtualnych. |

## <a name="jobs"></a>Zadania

| Operacja | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
| Anulowanie zadania | Anulowanie nie jest obsługiwane dla tego typu zadania — poczekaj, aż do zakończenia zadania. | Brak |
| Anulowanie zadania | Zadanie nie jest w stanie cancelable - poczekaj, aż do zakończenia zadania. <br>LUB<br> Zaznaczonego zadania nie jest w stanie cancelable — poczekaj na ukończenie zadania.| Najprawdopodobniej niemal ukończenia zadania; Poczekaj, aż do zakończenia zadania |
| Anulowanie zadania | Nie można anulować zadania, ponieważ nie jest w toku — anulowanie jest obsługiwana tylko dla zadania, które są w toku. Anuluj próba w postępu zadania. | Dzieje się tak ze względu na stan przejściowy. Poczekaj chwilę i spróbuj ponownie operacji anulowania |
| Anulowanie zadania | Nie można anulować zadanie — poczekaj do czasu, gdy zakończy się zadanie. | Brak |


## <a name="restore"></a>Przywracanie
| Operacja | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
| Przywracanie | Przywracanie nie powiodło się z chmurą wewnętrzny błąd | <ol><li>Usługa w chmurze do którego chcesz przywrócić skonfigurowano ustawienia DNS. Możesz sprawdzić <br>$deployment = get AzureDeployment - NazwaUsługi "NazwaUsługi"-przedział "Produkcji" Get AzureDns - DnsSettings $deployment. DnsSettings<br>W przypadku adresu skonfigurowanego, oznacza to, czy ustawienia DNS są skonfigurowane.<br> <li>Usługa w chmurze do której chcesz przywrócić skonfigurowano ReservedIP i istniejące maszyny wirtualne w usłudze w chmurze są w stanie zatrzymania.<br>Można sprawdzić, czy usługi w chmurze została zarezerwowana IP, korzystając z następujących poleceń cmdlet środowiska powershell:<br>$deployment = get AzureDeployment - NazwaUsługi "NazwaUsługi"-dep. przedział $ "Produkcji" ReservedIPName <br><li>Chcesz przywrócić maszyny wirtualnej z następujących konfiguracji sieci specjalnych w tym samym usługi w chmurze. <br>-Maszyn wirtualnych w obszarze konfigurację usługi równoważenia obciążenia (wewnętrzne i zewnętrzne)<br>-Maszyn wirtualnych z wielu zastrzeżone adresy IP<br>-Maszyn wirtualnych z kart<br>Wybierz nowe usługi w chmurze w interfejsie użytkownika lub zajrzyj do [przywracania zagadnienia](./backup-azure-arm-restore-vms.md/#restoring-vms-with-special-network-configurations) dla maszyny wirtualne z konfiguracji sieci specjalnych</ol> |
| Przywracanie | Wybrane nazwy DNS nie jest już zajęty — Określ inną nazwę DNS i spróbuj ponownie. | Poniżej nazwy DNS odwołuje się do nazwy usługi cloud (zazwyczaj kończące się literami. cloudapp.net). Musi to być unikatowe. Jeśli wystąpi ten błąd, należy wybrać inną nazwę maszyn wirtualnych podczas przywracania. <br><br> Zauważ, że ten błąd jest wyświetlana tylko dla użytkowników Azure portal. Przywracanie przy użyciu programu PowerShell powiedzie, ponieważ tylko przywraca dysków i nie zostanie utworzona maszyn wirtualnych. Komunikat o błędzie będzie wypadek, jeśli maszyn wirtualnych jawnie zostanie utworzone przez Ciebie po operacji przywracania dysku. |
| Przywracanie | Konfiguracja określonego wirtualną sieć nie jest prawidłowa — Określ konfigurację innego wirtualną sieć i spróbuj ponownie. | Brak |
| Przywracanie | Usługa w chmurze określonej używa zastrzeżony adres IP, które nie odpowiadają konfiguracji maszyny wirtualnej Trwa przywracanie — Określ usługi w chmurze różnych, która nie korzysta z zastrzeżony adres IP lub wybrać inny punkt odzyskiwania, aby przywrócić z. | Brak |
| Przywracanie | Usługa w chmurze osiągnięto limit liczby wprowadzania punkty końcowe — ponowić próbę, określając usługi w chmurze różnych lub za pomocą istniejący punkt końcowy. | Brak |
| Przywracanie | Kopii zapasowej konta miejsca do magazynowania magazynu i docelowej są dostępne w dwóch różnych regionów — upewnij się, czy konto miejsca do magazynowania w operacji przywracania jest w tym samym regionie Azure jako kopii zapasowej magazynu. | Brak |
| Przywracanie | Konto miejsca do magazynowania określona dla operacji przywracania nie jest obsługiwane — tylko Basic i standardowe kont miejsca do magazynowania z lokalnie zbędne lub geo replikacji zbędne ustawienia są obsługiwane. Wybierz konto obsługiwane miejsca do magazynowania | Brak |
| Przywracanie | Typ konta miejsca do magazynowania określone dla operacji przywracania nie jest w trybie online — upewnij się, że konta miejsca do magazynowania, określonego w operacji przywracania jest w trybie online | Może się to zdarzyć z powodu błędu przejściowych w magazynie Azure lub z powodu awarii. Wybierz inne konto miejsca do magazynowania. |
| Przywracanie | Osiągnięto przydziału Grupa zasobów — usuwanie kilka grup zasobów z Azure portal lub skontaktuj się z obsługą Azure, aby zwiększyć limity. | Brak |
| Przywracanie | Nie istnieje zaznaczonej podsieci — wybierz podsieci, która istnieje | Brak |


## <a name="policy"></a>Zasady
| Operacja | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
| Tworzenie zasad | Nie można utworzyć zasady - Zmniejsz opcji przechowywania, aby kontynuować konfiguracji zasad. | Brak |


## <a name="vm-agent"></a>Agent maszyn wirtualnych

### <a name="setting-up-the-vm-agent"></a>Aby skonfigurować agenta maszyn wirtualnych
Agent maszyn wirtualnych jest zazwyczaj zawarte w maszyny wirtualne, które zostały utworzone z galerii Azure. Jednak maszyn wirtualnych, które są migrowane z centrami danych lokalnych nie ma agenta maszyn wirtualnych zainstalowany. Takie maszyny wirtualne agenta maszyn wirtualnych musi być zainstalowany jawnie. Dowiedz się więcej o [instalowaniu agenta maszyn wirtualnych na istniejące maszyn wirtualnych](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Dla maszyny wirtualne systemu Windows:

- Pobierz i zainstaluj [agenta MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Konieczne będzie uprawnienia administratora, aby ukończyć instalację.
- [Aktualizowanie właściwości maszyn wirtualnych](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) oznaczającą, że agent jest zainstalowany.

Dla maszyny wirtualne Linux:

- Zainstaluj najnowszą wersję [agenta Linux](https://github.com/Azure/WALinuxAgent) z github.
- [Aktualizowanie właściwości maszyn wirtualnych](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) oznaczającą, że agent jest zainstalowany.


### <a name="updating-the-vm-agent"></a>Aktualizowanie agenta maszyn wirtualnych
Dla maszyny wirtualne systemu Windows:

- Aktualizowanie agenta maszyn wirtualnych jest tak proste, jak ponownie zainstalować [plików binarnych agenta maszyn wirtualnych](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Jednak należy się upewnić, czy nie wykonywanie kopii zapasowej jest uruchomiony Agent maszyn wirtualnych się podczas aktualizacji.

Dla maszyny wirtualne Linux:

- Postępuj zgodnie z instrukcjami na [Aktualizowanie agenta maszyn wirtualnych Linux](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>Sprawdzanie poprawności instalacji agenta maszyn wirtualnych
Jak sprawdzić, czy wersja agenta maszyn wirtualnych na maszyny wirtualne systemu Windows:

1. Zaloguj się Azure maszyn wirtualnych i przejdź do folderu *C:\WindowsAzure\Packages*. Powinien znajdować się plik WaAppAgent.exe Prezentuj.
2. Kliknij prawym przyciskiem myszy plik, przejdź do **Właściwości**, a następnie wybierz kartę **Szczegóły** . Pole wersji produktu należy 2.6.1198.718 lub nowszy

## <a name="troubleshoot-vm-snapshot-issues"></a>Rozwiązywanie problemów migawki maszyn wirtualnych
Wykonywanie kopii zapasowych maszyn wirtualnych zależy od tego, wydanie polecenia migawkę do podstawowej magazynu. Nie ma dostępu do miejsca do magazynowania lub opóźnienia migawkę na wykonanie zadania może się nie powieść kopii zapasowej. Następujące czynności mogą powodować niepowodzenie zadania migawki.

1. Dostęp do sieci do magazynu jest zablokowane za pomocą NSG<br>
   Dowiedz się więcej na temat [Włączanie dostępu do sieci](backup-azure-vms-prepare.md#2-network-connectivity) przy użyciu albo WhiteListing od miejsca do magazynowania lub za pośrednictwem serwera proxy.
2.  Maszyny wirtualne z kopią zapasową programu Sql Server skonfigurowane może spowodować opóźnienie zadania migawki <br>
    Domyślnie maszyn wirtualnych utworzyć kopię zapasową problemów VSS pełnej kopii zapasowej na maszyny wirtualne systemu Windows. Na maszyny wirtualne, które są uruchomione serwery Sql Server i Sql Server jest skonfigurowany kopii zapasowej może to spowodować opóźnienie wykonanie migawkę. Ustaw po klucza rejestru wystąpienia awarii kopii zapasowej z powodu problemów z migawki.

    ```
    [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
    "USEVSSCOPYBACKUP"="TRUE"
    ```
3.  Stan maszyn wirtualnych zgłoszone niepoprawnie, ponieważ maszyn wirtualnych zostanie zamknięty w RDP.  <br>
    Jeśli masz zamknięty maszyny wirtualnej RDP, sprawdź, czy w portalu że maszyn wirtualnych status jest uwzględniany poprawnie. Jeśli nie, zamknij maszyn wirtualnych w portalu za pomocą opcji "Zamknięcie" na pulpicie nawigacyjnym maszyn wirtualnych.
4.  Jeśli więcej niż cztery maszyn wirtualnych Udostępnij samo chmury usługi, należy skonfigurować wielu kopii zapasowej zasad etapu czasy tworzenia kopii zapasowych tak bez, więcej niż cztery maszyn wirtualnych kopie zapasowe są uruchamiane w tym samym czasie. Spróbuj rozsunąć czasu od siebie między zasady godziny rozpoczęcia kopii zapasowej. 
5.  Maszyn wirtualnych działa na Procesora i pamięci wysokiej.<br>
    Jeśli maszyna wirtualna działa na wysoki Procesora usage(>90%) lub pamięci, migawki zadanie jest w kolejce, opóźnione i zostanie po pewnym czasie otrzymuje Przekroczono limit. Spróbuj kopii zapasowej na żądanie w takiej sytuacji.

<br>

## <a name="networking"></a>Sieci
Podobnie jak wszystkie rozszerzenia rozszerzenia kopii zapasowej muszą mieć dostęp do publicznego Internetu do pracy. Nie ma dostępu do publicznego Internetu można pojawiają się na różne sposoby:

- Rozszerzenie Instalacja kończy się niepowodzeniem
- Może się nie powieść kopii zapasowych (na przykład migawki dysku)
- Wyświetlanie stanu wykonywanie kopii zapasowej może zakończyć się niepowodzeniem

Połączone przegubowo konieczności rozwiązywania publicznych adresów internetowych [w tym miejscu](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Będzie konieczne sprawdzanie konfiguracji systemu DNS dla VNET i upewnij się, że identyfikatory URI Azure będzie możliwe.

Po poprawnie zakończeniu rozpoznawanie nazw dostęp do adresy IP Azure również musi być udostępniana. Aby odblokować dostępu do infrastruktury Azure, wykonaj jedną z następujących czynności:

1. Listy sprawdzonej Azure centrum danych zakresów adresów IP.
    - Uzyskiwanie listy systemu [Azure centrum danych adresy IP](https://www.microsoft.com/download/details.aspx?id=41653) należy whitelisted.
    - Wyłączanie blokowania adresy IP, przy użyciu polecenia cmdlet [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) . Uruchom to polecenie cmdlet w Głosowa Azure, w oknie programu PowerShell podwyższonym poziomem uprawnień (polecenie Uruchom jako Administrator).
    - Dodawanie reguły do NSG (Jeśli masz w miejscu) umożliwia dostęp do adresy IP.
2. Utwórz ścieżkę na ruch HTTP
    - Jeśli masz kilka ograniczeń sieci w miejscu (sieć grupy zabezpieczeń, na przykład) wdrożyć serwer proxy HTTP, aby skierować ruch. Czynności, aby wdrożyć serwer HTTP Proxy można znaleźć [w tym miejscu](backup-azure-vms-prepare.md#2-network-connectivity).
    - Dodawanie reguły do NSG (Jeśli masz w miejscu) umożliwiają dostęp do INTERNETU z serwera HTTP Proxy.

>[AZURE.NOTE] DHCP musi być włączona wewnątrz gościa do tworzenia kopii zapasowych IaaS maszyn wirtualnych do pracy.  Jeśli potrzebujesz statyczny adres IP prywatne, należy skonfigurować go przy użyciu platformy. Opcja DHCP wewnątrz maszyn wirtualnych powinna być włączona po lewej.
Wyświetl więcej informacji o [ustawianiu statyczny adres IP prywatne wewnętrzny](../virtual-network/virtual-networks-reserved-private-ip.md).
