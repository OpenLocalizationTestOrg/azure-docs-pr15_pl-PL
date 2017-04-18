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

## <a name="discovery"></a>Odnajdowanie

| Wykonywanie kopii zapasowej | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
| Odnajdowanie | Nie można rozpoznać nowe elementy - napotkał Microsoft Azure wykonywanie kopii zapasowych i błąd wewnętrzny. Poczekaj kilka minut, a następnie spróbuj ponownie. | Ponów próbę procesu odnajdowania po 15 minut.
| Odnajdowanie | Nie można rozpoznać nowych elementów — innego odnajdowania operacja jest już w toku. Poczekaj, aż bieżącej operacji odnajdowania zostało ukończone. | Brak |

## <a name="register"></a>Zarejestruj się
| Wykonywanie kopii zapasowej | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
| Zarejestruj się | Liczba dyski danych dołączone maszyn wirtualnych przekroczyła obsługiwany limit - odłączanie niektóre dyski danych na tym komputerze wirtualnych i ponowić próbę. Kopia zapasowa Azure obsługuje maksymalnie 16 dyski danych dołączone do Azure maszyn wirtualnych do tworzenia kopii zapasowych | Brak |
| Zarejestruj się | Kopia zapasowa Microsoft Azure wystąpił błąd wewnętrzny - Czekaj na kilka minut i spróbuj ponownie wykonać operację. Jeśli problem będzie nadal występował, skontaktuj się Support firmy Microsoft. | Zostanie wyświetlony ten błąd z jednej z następujących konfiguracji nieobsługiwane maszyn wirtualnych na Premium LRS. <br> Magazyn Premium maszyny wirtualne kopię zapasową można przy użyciu magazynu usługi odzyskiwania. [Dowiedz się więcej](backup-introduction-to-azure-backup.md/#back-up-and-restore-premium-storage-vms) |
| Zarejestruj się | Rejestracja nie powiodła się limit czasu operacji zainstalować agenta | Sprawdź, jeśli wersja systemu operacyjnego maszyny wirtualnej jest obsługiwana. |
| Zarejestruj się | Wykonanie polecenia nie powiodło się — inna operacja jest w toku dla tego elementu. Poczekaj, aż do zakończenia poprzedniej operacji | Brak |
| Zarejestruj się | Maszyn wirtualnych o wirtualnych dyskach twardych przechowywane w magazynie Premium nie są obsługiwane przez wykonywanie kopii zapasowych | Brak |
| Zarejestruj się | Agent maszyn wirtualnych nie znajduje się na komputerze wirtualnych - Zainstaluj wymaganego wymagania wstępne agenta maszyn wirtualnych, a następnie ponownie uruchomić operację. | [Dowiedz się więcej](#vm-agent) o instalacji agenta maszyn wirtualnych i sprawdzania poprawności instalacji agenta maszyn wirtualnych. |

## <a name="backup"></a>Wykonywanie kopii zapasowych

| Wykonywanie kopii zapasowej | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
| Wykonywanie kopii zapasowych | Nie można komunikować się z agentem maszyn wirtualnych migawkę stanu. Przekroczono limit czasu migawki maszyn wirtualnych sub zadania. — Zobacz przewodnik rozwiązywania problemów na temat rozwiązać ten problem. | Ten błąd jest generowany, jeśli występuje problem z agentem maszyn wirtualnych lub dostęp do sieci Azure infrastruktury jest zablokowany w inny sposób. Dowiedz się więcej na temat [debugowania w górę migawki maszyn wirtualnych problemy](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Jeśli agenta maszyn wirtualnych nie powoduje problemy, a następnie ponownie uruchom maszyn wirtualnych. W czasie niepoprawnym stanie maszyn wirtualnych może powodować problemy z i ponowne uruchamianie maszyn wirtualnych zresetowanie ten "złym stanie" |
| Wykonywanie kopii zapasowych | Wykonywanie kopii zapasowych nie powiodło się ze względu na błąd wewnętrzny — ponów operację w ciągu kilku minut. Jeśli problem będzie nadal występował, skontaktuj się z pomocą techniczną firmy Microsoft Support | Sprawdź, czy występuje problem przejściowych w dostępie do magazynowania maszyn wirtualnych. Sprawdź [Stan Azure](https://azure.microsoft.com/en-us/status/) , aby sprawdzić, czy jest w toku kwestie związane z obliczeń i miejsca do magazynowania i sieci w regionie. Sprawdź ponów próbę problem kopii zapasowej wpis jest nieco ograniczane. |
| Wykonywanie kopii zapasowych | Nie można wykonać operacji, ponieważ istnieje już maszyn wirtualnych. | Nie można wykonać kopii zapasowej, jeśli maszyn wirtualnych, skonfigurowane dla kopii zapasowej zostały usunięte. Zatrzymaj dalsze kopie zapasowe, przejść do widoku chronionego elementy, zaznacz element chronionego i kliknij pozycję Zatrzymaj ochronę. Można zachować danych przez wybranie opcji przechowywania kopii zapasowych danych. Możesz później wznowić ochrony dla tej maszyny wirtualnej, klikając na konfigurowanie ochrony w widoku zarejestrowana elementy|
| Wykonywanie kopii zapasowych | Nie można zainstalować usługi odzyskiwania Azure rozszerzenie zaznaczonego elementu - Agent maszyn wirtualnych jest wstępnie wymagane dla rozszerzenia usług Azure odzyskiwania. Zainstaluj agenta maszyn wirtualnych Azure i ponownie uruchomić operację rejestracji | <ol> <li>Sprawdź, jeśli agenta maszyn wirtualnych została zainstalowana poprawnie. <li>Upewnij się, czy flaga konfiguracji maszyn wirtualnych jest ustawiona prawidłowo.</ol> [Dowiedz się więcej](#validating-vm-agent-installation) o instalacji agenta maszyn wirtualnych i sprawdzania poprawności instalacji agenta maszyn wirtualnych. |
| Wykonywanie kopii zapasowych | Wykonanie polecenia nie powiodło się — inna operacja jest w toku dla tego elementu. Poczekaj, aż do zakończenia poprzedniej operacji, a następnie ponów próbę | Istniejącą kopię zapasową lub Przywróć zadanie maszyn wirtualnych jest uruchomiony, a nie można uruchomić nowe zadanie, gdy jest uruchomiona istniejące zadanie. |
| Wykonywanie kopii zapasowych | Rozszerzenie Instalacja nie powiodła się z powodu błędu "COM + nie może skontaktować się z koordynatorem transakcji Microsoft Distributed | Zazwyczaj oznacza to, że Usługa COM + nie działa. Kontakt z pomocą techniczną firmy Microsoft, aby uzyskać pomoc dotyczącą rozwiązywania tego problemu. |
| Wykonywanie kopii zapasowych | Migawka niepowodzenie operacji VSS operacji "ten dysk jest zablokowany przez szyfrowanie dysków funkcją BitLocker. Musisz odblokować ten dysk z poziomu Panelu sterowania. | Wyłącz funkcję BitLocker dla wszystkich dysków na maszyn wirtualnych i obserwować, jeśli VSS problemu |
| Wykonywanie kopii zapasowych | Maszyn wirtualnych o wirtualnych dyskach twardych przechowywane w magazynie Premium nie są obsługiwane przez wykonywanie kopii zapasowych | Brak |
| Wykonywanie kopii zapasowych | Nie można odnaleźć Azure maszyn wirtualnych. | Dzieje się tak, gdy podstawowy maszyn wirtualnych zostanie usunięty, ale politykę wykonywania kopii zapasowych w dalszym ciągu poszukaj maszyn wirtualnych do wykonywania kopii zapasowych. Aby naprawić ten błąd: <ol><li>Ponowne tworzenie maszyny wirtualnej z tej samej nazwie i tym samym Nazwa grupy zasobów [nazwa usługi cloud], <br>(LUB) <li> Wyłączyć ochronę tego maszyn wirtualnych, dzięki czemu kolejne kopie zapasowe nie będą uzyskiwanie wyzwolone. </ol> |
| Wykonywanie kopii zapasowych | Agent maszyn wirtualnych nie znajduje się na komputerze wirtualnych — Zainstaluj wymaganego wymagania wstępne agenta maszyn wirtualnych, a następnie ponownie uruchomić operację. | [Dowiedz się więcej](#vm-agent) o instalacji agenta maszyn wirtualnych i sprawdzania poprawności instalacji agenta maszyn wirtualnych. |

## <a name="jobs"></a>Zadania
| Operacja | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
| Anulowanie zadania | Anulowanie nie jest obsługiwane dla tego typu zadania — poczekaj, aż do zakończenia zadania. | Brak |
| Anulowanie zadania | Zadanie nie jest w stanie cancelable — poczekaj, aż do zakończenia zadania. <br>LUB<br> Wybranego zadania nie jest w stanie cancelable — poczekaj na ukończenie zadania.| Najprawdopodobniej niemal ukończenia zadania; Poczekaj, aż do zakończenia zadania |
| Anulowanie zadania | Nie można anulować zadania, ponieważ nie jest w toku — anulowanie jest obsługiwana tylko dla zadania, które są w toku. Anuluj próba w postępu zadania. | Dzieje się tak ze względu na stan przejściowy. Poczekaj chwilę i spróbuj ponownie operacji anulowania |
| Anulowanie zadania | Nie można anulować zadanie — poczekaj, dopóki nie zakończy się zadanie. | Brak |


## <a name="restore"></a>Przywracanie
| Operacja | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
| Przywracanie | Przywracanie nie powiodło się z chmurą wewnętrzny błąd | <ol><li>Usługa w chmurze do którego chcesz przywrócić skonfigurowano ustawienia DNS. Możesz sprawdzić <br>$deployment = get AzureDeployment - NazwaUsługi "NazwaUsługi"-przedział "Produkcji" Get AzureDns - DnsSettings $deployment. DnsSettings<br>W przypadku adresu skonfigurowanego, oznacza to, czy ustawienia DNS są skonfigurowane.<br> <li>Usługa w chmurze do której chcesz przywrócić skonfigurowano ReservedIP i istniejące maszyny wirtualne w usłudze w chmurze są w stanie zatrzymania.<br>Można sprawdzić, czy usługi w chmurze została zarezerwowana IP, korzystając z następujących poleceń cmdlet środowiska powershell:<br>$deployment = get AzureDeployment - NazwaUsługi "NazwaUsługi"-dep. przedział $ "Produkcji" ReservedIPName <br><li>Chcesz przywrócić maszyny wirtualnej z następujących konfiguracji sieci specjalnych w tym samym usługi w chmurze. <br>-Maszyn wirtualnych w obszarze konfigurację usługi równoważenia obciążenia (wewnętrzne i zewnętrzne)<br>-Maszyn wirtualnych z wielu zastrzeżone adresy IP<br>-Maszyn wirtualnych z kart<br>Wybierz nowe usługi w chmurze w interfejsie użytkownika lub zajrzyj do [przywracania zagadnienia](./backup-azure-restore-vms.md/#restoring-vms-with-special-network-configurations) dla maszyny wirtualne z konfiguracji sieci specjalnych</ol> |
| Przywracanie | Wybrane nazwy DNS nie jest już zajęty — Określ inną nazwę DNS i spróbuj ponownie. | Poniżej nazwy DNS odwołuje się do nazwy usługi cloud (zazwyczaj kończące się literami. cloudapp.net). Musi to być unikatowe. Jeśli wystąpi ten błąd, należy wybrać inną nazwę maszyn wirtualnych podczas przywracania. <br><br> Ten błąd jest wyświetlana tylko dla użytkowników Azure portal. Przywracanie przy użyciu programu PowerShell zakończyło się powodzeniem, ponieważ tylko przywraca dysków i nie zostanie utworzona maszyn wirtualnych. Komunikat o błędzie będzie wypadek, jeśli maszyn wirtualnych jawnie zostanie utworzone przez Ciebie po operacji przywracania dysku. |
| Przywracanie | Konfiguracja określonego wirtualną sieć nie jest prawidłowa — Określ konfigurację innego wirtualną sieć i spróbuj ponownie. | Brak |
| Przywracanie | Usługa w chmurze określonej używa zastrzeżony adres IP, które nie odpowiadają konfiguracji maszyny wirtualnej Trwa przywracanie — Określ usługi w chmurze różnych, która nie korzysta z zastrzeżony adres IP lub wybrać inny punkt odzyskiwania, aby przywrócić z. | Brak |
| Przywracanie | Usługa w chmurze osiągnięto limit liczby wprowadzania punkty końcowe — ponowić próbę, określając usługi w chmurze różnych lub za pomocą istniejący punkt końcowy. | Brak |
| Przywracanie | Kopii zapasowej konta miejsca do magazynowania magazynu i docelowej są dostępne w dwóch różnych regionów — upewnij się, czy konto miejsca do magazynowania w operacji przywracania jest w tym samym regionie Azure jako kopii zapasowej magazynu. | Brak |
| Przywracanie | Konto miejsca do magazynowania określona dla operacji przywracania nie jest obsługiwane - tylko Basic i standardowe kont miejsca do magazynowania z lokalnie zbędne lub geo replikacji zbędne ustawienia są obsługiwane. Wybierz konto obsługiwane miejsca do magazynowania | Brak |
| Przywracanie | Typ konta miejsca do magazynowania określone dla operacji przywracania nie jest w trybie online — upewnij się, że konta miejsca do magazynowania, określonego w operacji przywracania jest w trybie online | Może się to zdarzyć z powodu błędu przejściowych w magazynie Azure lub z powodu awarii. Wybierz inne konto miejsca do magazynowania. |
| Przywracanie | Osiągnięto przydziału Grupa zasobów — usuwanie kilka grup zasobów z Azure portal lub skontaktuj się z obsługą Azure, aby zwiększyć limity. | Brak |
| Przywracanie | Nie istnieje zaznaczonej podsieci — wybierz podsieci, która istnieje | Brak |


## <a name="policy"></a>Zasady
| Operacja | Informacje o błędzie | Obejście |
| -------- | -------- | -------|
| Tworzenie zasad | Nie można utworzyć zasady — Zmniejsz opcji przechowywania, aby kontynuować konfiguracji zasad. | Brak |


## <a name="vm-agent"></a>Agent maszyn wirtualnych

### <a name="setting-up-the-vm-agent"></a>Aby skonfigurować agenta maszyn wirtualnych
Zazwyczaj agenta maszyn wirtualnych już występuje maszyny wirtualne, które zostały utworzone z galerii Azure. Jednak maszyn wirtualnych, które są migrowane z centrami danych lokalnych nie ma agenta maszyn wirtualnych zainstalowany. Takie maszyny wirtualne Agent maszyn wirtualnych musi być zainstalowany jawnie. Dowiedz się więcej o [instalowaniu agenta maszyn wirtualnych na istniejące maszyn wirtualnych](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Dla maszyny wirtualne systemu Windows:

- Pobierz i zainstaluj [agenta MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Konieczne będzie uprawnienia administratora, aby ukończyć instalację.
- [Aktualizowanie właściwości maszyn wirtualnych](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) oznaczającą, że agent jest zainstalowany.

Dla maszyny wirtualne Linux:

- Zainstaluj najnowszą wersję [agenta Linux](https://github.com/Azure/WALinuxAgent) z github.
- [Aktualizowanie właściwości maszyn wirtualnych](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) , aby wskazać, że agent jest zainstalowany.


### <a name="updating-the-vm-agent"></a>Aktualizowanie agenta maszyn wirtualnych
Dla maszyny wirtualne systemu Windows:

- Aktualizowanie agenta maszyn wirtualnych jest tak proste, jak ponownie zainstalować [plików binarnych agenta maszyn wirtualnych](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Jednak należy się upewnić, czy nie wykonywanie kopii zapasowej jest uruchomiony Agent maszyn wirtualnych się podczas aktualizacji.

Dla maszyny wirtualne Linux:

- Postępuj zgodnie z instrukcjami na [Aktualizowanie agenta maszyn wirtualnych Linux](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>Sprawdzanie poprawności instalacji agenta maszyn wirtualnych
Jak sprawdzić, czy wersja agenta maszyn wirtualnych na maszyny wirtualne systemu Windows:

1. Zaloguj się Azure maszyn wirtualnych i przejdź do folderu *C:\WindowsAzure\Packages*. Powinien znajdować się plik WaAppAgent.exe Prezentuj.
2. Kliknij prawym przyciskiem myszy plik, przejdź do **Właściwości**, a następnie wybierz kartę **Szczegóły** . Pole wersji produktu należy 2.6.1198.718 lub nowszy





