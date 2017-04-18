<properties
    pageTitle="Szczegółowe zdalnego Rozwiązywanie problemów z pulpitem | Microsoft Azure"
    description="Przeglądanie szczegółowych instrukcji rozwiązywania problemów zdalnego pulpitu błędów, której nie można do maszyn wirtualnych systemu Windows, platformy Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"
    keywords="Nie można połączyć do pulpitu zdalnego, rozwiązywanie problemów z pulpitu zdalnego, pulpitu zdalnego nie można połączyć błędy pulpitu zdalnego, rozwiązywanie problemów pulpitu zdalnego, problemy z pulpitu zdalnego
"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Szczegółowe kroki rozwiązywania problemów Podłączanie pulpitu zdalnego maszyny wirtualne Windows platformy Azure

W tym artykule opisano szczegółowe kroki rozwiązywania problemów diagnozowanie i rozwiązywanie złożonych błędy pulpitu zdalnego dla systemu Windows Azure maszyn wirtualnych.

> [AZURE.IMPORTANT] Aby wyeliminować najbardziej typowych błędów pulpitu zdalnego, upewnij się przeczytać [podstawowe artykuł dotyczący rozwiązywania problemów dla pulpitu zdalnego](virtual-machines-windows-troubleshoot-rdp-connection.md) przed kontynuowaniem.

Może się pojawić komunikat o błędzie pulpitu zdalnego, który wygląda inaczej niż dowolną określone komunikaty o błędach objęte [podstawowe pulpitu zdalnego przewodnik rozwiązywania problemów](virtual-machines-windows-troubleshoot-rdp-connection.md). Wykonaj poniższe czynności, aby określić, dlaczego nie można połączyć się z usługą RDP na maszyn wirtualnych Azure jest klienta Remote Desktop (RDP).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, można skontaktować się Azure ekspertów na [MSDN Azure i fora przepełnienie stosu](https://azure.microsoft.com/support/forums/). Ponadto można również pliku Azure pomocy technicznej zdarzenia. Przejdź do [witryny pomocy technicznej Azure](https://azure.microsoft.com/support/options/) , a następnie kliknij przycisk **Uzyskiwanie pomocy technicznej**. Aby uzyskać informacje o korzystaniu z platformy Azure pomocy technicznej, przeczytaj [Często zadawane pytania dotyczące programu Microsoft Azure pomocy technicznej](https://azure.microsoft.com/support/faq/).


## <a name="components-of-a-remote-desktop-connection"></a>Składniki połączenia pulpitu zdalnego

Połączenie RDP obejmuje następujące składniki:

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_0.png)

Przed kontynuowaniem warto przejrzeć umysłowo, co zostało zmienione od czasu ostatniego pomyślnego połączenia pulpitu zdalnego do maszyn wirtualnych. Na przykład:

- Publiczny adres IP maszyn wirtualnych lub usług w chmurze zawierające maszyn wirtualnych (zwanych również wirtualny adres IP [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) został zmieniony. Błąd RDP prawdopodobnie pamięć podręczną DNS klienta nadal jest *stary adres IP* zarejestrowany dla nazwy DNS. Opróżnij pamięć podręczną DNS klienta i spróbuj ponownie nawiązać połączenie maszyn wirtualnych. Lub spróbuj nawiązać połączenie bezpośrednio z nowego VIP.
- Korzystasz z aplikacji innych firm do zarządzania połączeniami pulpitu zdalnego zamiast przy użyciu połączenia wygenerowane przez Azure portal. Upewnij się, że konfiguracja aplikacji zawiera poprawny port TCP dla ruchu pulpitu zdalnego. Możesz sprawdzić tego portu dla klasyczny maszyny wirtualnej w [Azure portal](https://portal.azure.com), klikając przycisk Ustawienia maszyn > punkty końcowe.


## <a name="preliminary-steps"></a>Czynności wstępne

Przed przejściem do szczegółowej procedury rozwiązywania problemów,

- Sprawdź stan maszyny wirtualnej w portalu klasyczny Azure lub portal Azure na temat wszelkich problemów oczywiste.
- Wykonaj [Szybki fix instrukcje dotyczące typowych błędów RDP w podręczniku podstawowe rozwiązywania problemów](virtual-machines-windows-troubleshoot-rdp-connection.md#quick-troubleshooting-steps).


Spróbuj ponownie połączyć się maszyn wirtualnych za pośrednictwem pulpitu zdalnego po wykonaniu tych kroków.


## <a name="detailed-troubleshooting-steps"></a>Uzyskać szczegółowe instrukcje dotyczące rozwiązywania problemów

Klienta pulpitu zdalnego może nie być nawiązywać połączenia z usługami pulpitu zdalnego na maszyn wirtualnych Azure z powodu problemów w następujących źródeł:

- [Komputer kliencki pulpitu zdalnego](#source-1-remote-desktop-client-computer)
- [Urządzenia intranecie organizacji](#source-2-organization-intranet-edge-device)
- [Punkt końcowy usługi w chmurze i uzyskać dostęp do listy kontroli (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Grupy zabezpieczeń sieci](#source-4-network-security-groups)
- [Systemu Windows Azure maszyn wirtualnych](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>Źródła 1: Komputer kliencki pulpitu zdalnego

Upewnij się, że na komputerze można nawiązywanie połączenia pulpitu zdalnego z innego lokalnego, komputer z systemem Windows.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_1.png)

Jeśli nie możesz sprawdzić następujące ustawienia na komputerze:

- Ustawienie zapory lokalnej, która blokuje ruchu pulpitu zdalnego.
- Zainstalowana lokalnie oprogramowanie klienckie serwera proxy, który uniemożliwia połączeń pulpitu zdalnego.
- Zainstalowana lokalnie monitorowania oprogramowania, które uniemożliwia połączeń pulpitu zdalnego sieci.
- Inne rodzaje oprogramowania zabezpieczającego które monitorowania ruchu lub zezwalanie lub niezezwalanie określone typy ruchu, który uniemożliwia połączeń pulpitu zdalnego.

W takich przypadkach tymczasowo wyłącz oprogramowanie i spróbuj połączyć się z komputerem lokalnym za pośrednictwem pulpitu zdalnego. Jeśli przyczynę rzeczywisty można znaleźć w ten sposób, Praca z administratorem sieci, aby poprawić ustawienia oprogramowania do zezwalania na połączenia pulpitu zdalnego.

## <a name="source-2-organization-intranet-edge-device"></a>Źródło 2: Urządzenia intranecie organizacji

Upewnij się, że komputer bezpośrednio połączony z Internetem mogą tworzyć połączenia pulpitu zdalnego usługi Azure maszyn wirtualnych.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_2.png)

Jeśli nie masz komputerze, na którym jest połączony bezpośrednio z Internetem, tworzenie i testowanie z nowego Azure maszyny wirtualnej w usłudze grupy lub w chmurze zasobów. Aby uzyskać więcej informacji zobacz [Tworzenie wirtualnych komputera z systemem Windows w Azure](virtual-machines-windows-hero-tutorial.md). Możesz usunąć maszyny wirtualnej i grupa zasobów lub usługi w chmurze, po teście.

Jeśli tworzysz połączenie pulpitu zdalnego z komputerem bezpośrednio podłączonego do Internetu, Sprawdź urządzenia krawędzi intranecie organizacji w celu:

- Wewnętrzna Zapora blokuje połączenia HTTPS z Internetem.
- Serwer proxy uniemożliwiają połączeń pulpitu zdalnego.
- Wykrywanie intruzów lub monitorowania oprogramowanie na urządzeniach w sieci krawędzi, który uniemożliwia połączeń pulpitu zdalnego sieci.

Praca z administratorem sieci, aby poprawić ustawienia urządzenia krawędzi intranecie organizacji, aby zezwolić oparte na HTTPS pulpitu zdalnego połączenia z Internetem.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Źródło 3: Punkt końcowy usługi w chmurze i list ACL

Dla maszyny wirtualne utworzony przy użyciu modelu wdrożenia klasyczny upewnij się, że inny maszyn wirtualnych Azure, który znajduje się w tym samym usługi w chmurze lub wirtualnej sieci mogą nawiązać połączenia pulpitu zdalnego maszyn wirtualnych usługi Azure.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_3.png)

> [AZURE.NOTE] Dla maszyn wirtualnych utworzone w Menedżerze zasobów, przejdź do [źródła 4: grup zabezpieczeń sieci](#source-4-network-security-groups).

Jeśli nie masz innego komputera wirtualnych w tym samym usługi w chmurze lub wirtualną sieć, utwórz je. Postępuj zgodnie z instrukcjami [Utwórz maszyny wirtualnej z systemem Windows w Azure](virtual-machines-windows-hero-tutorial.md). Po zakończeniu test, należy usunąć maszyny wirtualnej test.

Jeśli możesz połączyć za pośrednictwem pulpitu zdalnego maszyn wirtualnych w tym samym usługi w chmurze lub wirtualnej sieci, sprawdź następujące ustawienia:

- Konfiguracji punktu końcowego ruchu pulpitu zdalnego miejsca docelowego maszyn wirtualnych: prywatne port TCP punkt końcowy musi odpowiadać port TCP, na którym oczekuje maszyn wirtualnych usługi pulpitu zdalnego (wartość domyślna to 3389).
- Na liście ACL dla punktu końcowego ruch pulpitu zdalnego miejsca docelowego maszyn wirtualnych: ACL umożliwiają określenie dozwolone lub nie ruch przychodzący z Internetu na podstawie jego adresu IP źródła. Niepoprawnie skonfigurowane ACL zapobiec ruch przychodzący pulpitu zdalnego do punktu końcowego. Zaznacz do list ACL zapewnienie tego ruch przychodzący od usługi publicznych adresów IP swojego serwera proxy lub innych serwera granicznego jest dozwolone. Aby uzyskać więcej informacji, zobacz [Co to jest sieć listy kontroli dostępu (ACL)?](../virtual-network/virtual-networks-acl.md)

Aby sprawdzić, czy punkt końcowy jest źródłem problemu, Usuń bieżącą punktu końcowego i utworzyć nową Wybieranie portu losową z zakresu od 49152 — 65535 dla numeru portu zewnętrznego. Aby uzyskać więcej informacji zobacz [jak skonfigurować punkty końcowe maszyn wirtualnych](virtual-machines-windows-classic-setup-endpoints.md).

## <a name="source-4-network-security-groups"></a>Źródła 4: Grupy zabezpieczeń sieci

Grupy zabezpieczeń sieci umożliwiają większą kontrolę nad dozwolonych ruch przychodzący i wychodzący. Można tworzyć reguły obejmujące podsieci i usług w Azure wirtualną sieć w chmurze. Przejrzyj swoje reguły grupy zabezpieczeń sieci, aby upewnić się, że jest dozwolony ruch pulpitu zdalnego z Internetu:

- W portalu Azure Wybierz swojego maszyn wirtualnych
- Kliknij pozycję **wszystkie ustawienia** | **interfejsów sieciowych** i wybierz pozycję usługi sieciowej.
- Kliknij pozycję **wszystkie ustawienia** | **grupy zabezpieczeń sieci** i wybierz grupę zabezpieczeń sieci.
- Kliknij pozycję **wszystkie ustawienia** | **Reguły przychodzące zabezpieczeń** i upewnij się, masz regułę zezwalania RDP na porcie TCP 3389.
    - Jeśli nie masz regułę, kliknij przycisk **Dodaj** , aby utworzyć regułę. Wprowadź **port TCP** Protocol (protokół), a następnie **3389** zakresu portu docelowego.
    - Upewnij się, że akcja jest ustawiona na wartość **Zezwalaj** , a następnie kliknij przycisk OK, aby zapisać nowej reguły przychodzące.


Aby uzyskać więcej informacji, zobacz [Co to jest grupa zabezpieczeń sieci (NSG)?](../virtual-network/virtual-networks-nsg.md)

## <a name="source-5-windows-based-azure-vm"></a>Źródło 5: Systemu Windows Azure maszyn wirtualnych

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_5.png)

Sprawdź, czy błąd jest ze względu na samej maszyny wirtualnej Azure za pomocą [Azure IaaS (Windows) Diagnostyka pakietu](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) . Jeśli ten pakiet Diagnostyka nie może rozwiązać ten problem **RDP łączności z programem Azure maszyn wirtualnych (wymagane ponowne uruchomienie komputera)** , postępuj zgodnie z instrukcjami podanymi w [tym artykule](virtual-machines-windows-reset-rdp.md). W tym artykule resetuje usługi pulpitu zdalnego na komputerze wirtualnej:

- Włącz regułę domyślne "Pulpit zdalny" Zapora systemu Windows (TCP port 3389).
- Włączanie połączenia pulpitu zdalnego, ustawiając wartość rejestru HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections 0.

Ponownie spróbować się połączyć ze swojego komputera. Jeśli nadal nie możesz połączyć za pomocą pulpitu zdalnego, sprawdź następujące ewentualnym problemom:

- Usługa Pulpit zdalny nie jest uruchomiona maszyn wirtualnych miejsca docelowego.
- Usługi pulpitu zdalnego nie nasłuchują na TCP port 3389.
- Zapora systemu Windows lub innej zapory lokalnej ma reguły wychodzącej, który jest zapobieganie ruchu pulpitu zdalnego.
- Wykrywanie intruzów lub monitorowania oprogramowanie uruchomione na komputerze Azure wirtualnych sieci uniemożliwia połączeń pulpitu zdalnego.

Aby maszyny wirtualne utworzony przy użyciu modelu Klasyczny wdrożenia można używać zdalnej sesji programu PowerShell Azure Azure maszyn wirtualnych. Najpierw należy zainstalować certyfikat maszyny wirtualnej usłudze hostingu w chmurze. Przejdź do [Konfigurowanie bezpiecznego programu PowerShell dostępu zdalnego do maszyn wirtualnych Azure](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) i Pobierz plik skryptu **InstallWinRMCertAzureVM.ps1** na komputerze lokalnym.

Następnie zainstaluj Azure programu PowerShell, jeśli jeszcze tego nie zrobiono. Dowiedz się, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

Następnie otwórz okno wiersza polecenia programu PowerShell Azure i bieżący folder można zmienić lokalizację pliku skryptu **InstallWinRMCertAzureVM.ps1** . Aby uruchomić skrypt programu PowerShell Azure, należy ustawić zasady wykonywania poprawne. Uruchom polecenie **Get-ExecutionPolicy** , aby określić bieżący poziom zasad. Aby uzyskać informacje na temat ustawiania na odpowiedni poziom zobacz [Zestaw ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).

Następnie wprowadź nazwę subskrypcji Azure, nazwa usługi cloud i nazwy maszyn wirtualnych (usuwanie < i > znaki), a następnie uruchom następujące polecenia.

    $subscr="<Name of your Azure subscription>"
    $serviceName="<Name of the cloud service that contains the target virtual machine>"
    $vmName="<Name of the target virtual machine>"
    .\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName

Nazwa poprawne subskrypcji możesz przejść z właściwości _SubscriptionName_ ekran polecenia **Get-AzureSubscription** . Nazwa usługi cloud maszyny wirtualnej można uzyskać z kolumny _NazwaUsługi_ ekran polecenia **Get-AzureVM** .

Sprawdź, czy masz nowego certyfikatu. Otwieranie przystawki Certyfikaty dla bieżącego użytkownika, a w folderze **Trusted Root certyfikacji Authorities\Certificates** . Powinien zostać wyświetlony certyfikatu z nazwą DNS do usługi w chmurze w kolumnie wystawiony (przykład: cloudservice4testing.cloudapp.net).

Następnie rozpocznij zdalnej sesji programu PowerShell Azure za pomocą tych poleceń.

    $uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
    $creds = Get-Credential
    Enter-PSSession -ConnectionUri $uri -Credential $creds

Po wprowadzeniu poświadczeń administratora prawidłowy, powinien zostać wyświetlony tekst podobny do następującego monitu Azure programu PowerShell:

    [cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>

Pierwsza część tego monitu jest nazwę usługi cloud zawierający docelowej Głosowa, które mogą być inne niż "cloudservice4testing.cloudapp.net". Można teraz problemów Azure polecenia służące do tej usługi w chmurze, którzy chcą problemy wymienione i poprawianie konfiguracji programu PowerShell.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>Aby ręcznie poprawić usługami pulpitu zdalnego słuchanie TCP port

Jeśli nie można uruchomić [pakietu diagnostyki Azure IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) do wydania **RDP łączności z programem Azure maszyn wirtualnych (wymagane ponowne uruchomienie komputera)** , w wierszu zdalnej sesji programu PowerShell Azure, uruchom polecenie.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Właściwość PortNumber zawiera bieżący numer portu. W razie potrzeby zmień pulpitu zdalnego portu numer wstecz na wartość domyślną (3389) przy użyciu tego polecenia.

    Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389

Upewnij się, że port została zmieniona na 3389 za pomocą tego polecenia.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Zamknij zdalnej sesji programu PowerShell Azure za pomocą tego polecenia.

    Exit-PSSession

Sprawdź, czy punkt końcowy pulpitu zdalnego dla maszyn wirtualnych Azure także używa TCP port 3398 jako portu wewnętrznego. Uruchom maszyn wirtualnych Azure i spróbuj ponownie Podłączanie pulpitu zdalnego.


## <a name="additional-resources"></a>Dodatkowe zasoby

[Azure pakiet Diagnostyka IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[Jak zresetować hasło lub usługi pulpitu zdalnego dla maszyn wirtualnych systemu Windows](virtual-machines-windows-reset-rdp.md)

[Jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md)

[Rozwiązywanie problemów z połączenia Secure Shell (SSH) z systemem Linux maszyny wirtualnej Azure](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Rozwiązywanie problemów z dostępem do uruchamiania aplikacji na Azure maszyn wirtualnych](virtual-machines-linux-troubleshoot-app-connection.md)
