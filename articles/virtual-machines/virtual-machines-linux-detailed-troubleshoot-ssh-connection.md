<properties
    pageTitle="Szczegółowe SSH Rozwiązywanie problemów związanych z maszyn wirtualnych Azure | Microsoft Azure"
    description="Bardziej szczegółowe SSH czynności dla problemów dotyczących łączenia Azure maszyn wirtualnych rozwiązywania problemów"
    keywords="SSH odmowa połączenia, ssh błąd, azure ssh, SSH połączenie nie powiodło się"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="detailed-ssh-troubleshooting-steps"></a>Uzyskać szczegółowe instrukcje dotyczące rozwiązywania problemów SSH

Istnieje wiele możliwych przyczyn, które klienta SSH może nie być możliwe do osiągnięcia usługę SSH na maszyn wirtualnych. Jeśli zostały wykonane więcej [Ogólne kroki rozwiązywania problemów SSH](virtual-machines-linux-troubleshoot-ssh-connection.md), musisz rozwiązać problemu z połączeniem. Ten artykuł prowadzi użytkownika przez szczegółowych instrukcji rozwiązywania problemów, aby określić miejsce, w którym połączenie SSH kończy się niepowodzeniem i sposobu rozwiązania problemu.

## <a name="take-preliminary-steps"></a>Czynności wstępne

Na poniższym diagramie przedstawiono składniki, które wiążą się.

![Diagram przedstawiający części usługi SSH](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Poniższe czynności ułatwiają wyizolować źródła błędów i ustalanie rozwiązania.

Najpierw sprawdź stan maszyn wirtualnych w portalu.

W [portalu Azure](https://portal.azure.com):

1. W przypadku maszyny wirtualne utworzone przy użyciu modelu Klasyczny wdrożenia, wybierz opcję **Przeglądaj** > **maszyn wirtualnych (klasyczny)** > *nazwę maszyn wirtualnych*.

    -LUB-

    W przypadku maszyny wirtualne utworzone przy użyciu modelu Menedżera zasobów, wybierz opcję **Przeglądaj** > **maszyn wirtualnych** > *nazwę maszyn wirtualnych*.

    Okienko stan dla maszyn wirtualnych powinny być wyświetlane **uruchomiony**. Przewiń do ostatniej aktywności Pokaż do obliczeń, przechowywania i zasobów.

2. Wybierz **Ustawienia** , aby sprawdzić punkty końcowe, adresy i inne ustawienia.

    Aby zidentyfikować punkty końcowe w maszyny wirtualne, które zostały utworzone za pomocą Menedżera zasobów, sprawdź zdefiniowania [grupy zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md) . Również upewnij się, że zastosowano reguły do grupy zabezpieczeń sieci i że jest określany w podsieci.

W [portalu klasyczny Azure](https://manage.windowsazure.com)dla maszyny wirtualne, które zostały utworzone przy użyciu modelu Klasyczny wdrożenia:

1. Wybierz pozycję **maszyn wirtualnych** > *nazwę maszyn wirtualnych*.
2. Wybierz pozycję maszyn **pulpitu nawigacyjnego** , aby sprawdzić jego stan.
3. Wybierz **Monitor** , aby zobaczyć działanie ostatnio używane do obliczeń, przechowywania i zasobów.
4. Zaznacz **punkty końcowe** , aby upewnić się, że jest punkt końcowy dla ruchu SSH.

Aby sprawdzić połączenie sieciowe, sprawdź skonfigurowaną punkty końcowe i zobacz, jeśli zostanie wyświetlona maszyn wirtualnych przy użyciu protokołu innego, na przykład HTTP lub innej usługi.

Po wykonaniu tych kroków ponów próbę połączenia SSH.


## <a name="find-the-source-of-the-issue"></a>Znajdowanie źródła problemu

Klient SSH na komputerze może się nie powieść uzyskać dostęp do usługi SSH na maszyn wirtualnych Azure z powodu problemów i błędów konfiguracji w następujących czynności:

- [Komputer kliencki SSH](#source-1-ssh-client-computer)
- [Organizacja urządzenia](#source-2-organization-edge-device)
- [Punkt końcowy usługi w chmurze i uzyskać dostęp do listy kontroli (ACL)](#source-3-cloud-service-endpoint-and-acl)
- [Grupy zabezpieczeń sieci](#source-4-network-security-groups)
- [Systemem Linux Azure maszyn wirtualnych](#source-5-linux-based-azure-virtual-machine)

## <a name="source-1-ssh-client-computer"></a>Źródło 1: SSH komputer kliencki

Aby wyeliminować na komputerze jako źródła błędów, upewnij się, że można wprowadzić SSH połączenia z innym w lokalnym komputerze z systemem Linux.

![Diagram wyróżnienie SSH składniki komputera klienta](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Jeśli połączenie nie powiedzie się, sprawdź poniższe czynności na komputerze:

- Ustawienia zapory lokalnej, która blokuje ruchu SSH przychodzących i wychodzących (port TCP 22)
- Zainstalowana lokalnie oprogramowanie klienckie serwera proxy, który uniemożliwia SSH połączenia
- Zainstalowana lokalnie monitorowania oprogramowania, które uniemożliwia SSH połączeń sieci
- Inne rodzaje oprogramowania zabezpieczającego, które można monitorować ruch lub zezwalanie lub niezezwalanie określone typy ruchu

Jeśli jeden z tych warunków zastosować, tymczasowo wyłącz oprogramowanie i spróbuj połączenie SSH na komputerze lokalnym, aby dowiedzieć się, przyczyny, dla której połączenia są blokowane na komputerze. Następnie pracować z administratorem sieci, aby poprawić ustawienia oprogramowania do zezwalania na połączenia SSH.

Jeśli używasz certyfikatu uwierzytelniania sprawdzić, czy są te uprawnienia do folderu .ssh w katalogu macierzystego:

- ~/.Ssh chmod 700
- ~/.Ssh/ chmod 644\*pub
- ~/.Ssh/id_rsa chmod 600 (lub inne pliki, które mają przechowywanych w nich kluczy prywatnych)
- ~/.Ssh/known_hosts chmod 644 (zawiera hosts, które zostały połączone za pośrednictwem SSH)

## <a name="source-2-organization-edge-device"></a>Źródło 2: Urządzenia organizacji

Aby wyeliminować urządzenia krawędzi organizacji jako źródła błędów, upewnij się, że komputerze, na którym jest połączony bezpośrednio z Internetem mogą nawiązać połączenia SSH maszyn wirtualnych usługi Azure. Jeśli uzyskujesz dostęp do maszyn wirtualnych przez sieć VPN witryny do witryny lub połączenia Azure ExpressRoute, przejdź do [źródła 4: sieci grupy zabezpieczeń](#nsg).

![Diagram wyróżnienie urządzenia organizacji](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Jeśli nie masz na komputerze, na którym jest bezpośrednio połączony z Internetem, Utwórz nowe maszyny Azure w osobnej grupie zasobów lub usługa w chmurze i używać go. Aby uzyskać więcej informacji zobacz [Tworzenie maszyny wirtualnej systemem Linux platformy Azure](virtual-machines-linux-quick-create-cli.md). Usuwanie grupy zasobów lub maszyn wirtualnych i w chmurze, po zakończeniu testowania.

Jeśli tworzysz połączenie SSH z komputerem, który jest połączony bezpośrednio z Internetem, Sprawdź urządzenia krawędzi organizacji w celu:

- Wewnętrzna Zapora blokuje ruchu SSH z Internetem
- Serwer proxy, który uniemożliwia SSH połączenia
- Wykrywanie intruzów lub monitorowania oprogramowanie na urządzeniach w sieci krawędzi, który uniemożliwia SSH połączeń sieci

Praca z administratorem sieci, aby poprawić ustawienia urządzenia krawędzi organizacji zezwalanie na ruch SSH z Internetem.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Źródło 3: Punkt końcowy usługi w chmurze i list ACL

> [AZURE.NOTE] Tego źródła dotyczy tylko maszyny wirtualne, które zostały utworzone przy użyciu modelu Klasyczny wdrożenia. Dla maszyny wirtualne, które zostały utworzone za pomocą Menedżera zasobów, przejdź do [źródła 4: sieci grupy zabezpieczeń](#nsg).

Aby usunąć punktu końcowego usługi w chmurze i list ACL jako źródła błędu, sprawdź innego Azure maszyn wirtualnych w tej samej sieci wirtualnej można wprowadzić SSH połączenia do swojego maszyn wirtualnych.

![Diagram wyróżnienie punktu końcowego usługi w chmurze i listy kontroli dostępu](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Jeśli nie masz innego maszyn wirtualnych w tej samej sieci wirtualnej można łatwo tworzyć nowe. Aby uzyskać więcej informacji zobacz [Tworzenie maszyny Linux Azure za pomocą interfejsu wiersza polecenia](virtual-machines-linux-quick-create-cli.md). Usuwanie dodatkowego maszyn wirtualnych po wykonaniu tych czynności z testowania.

Jeśli tworzysz połączenie SSH z maszyn wirtualnych w tej samej sieci wirtualnej, wykonaj następujące czynności:

- **Konfiguracja punkt końcowy dla ruchu SSH miejsca docelowego maszyn wirtualnych.** Prywatne port TCP punktu końcowego powinny być zgodne port TCP, na którym oczekuje usługę SSH na maszyn wirtualnych. (Domyślny port jest 22). Maszyny wirtualne utworzone przy użyciu modelu wdrożenia Menedżera zasobów, sprawdź numer portu SSH TCP w portalu Azure, wybierając pozycję **Przeglądaj** > **maszyn wirtualnych (wersja 2)** > *nazwę maszyn wirtualnych* > **Ustawienia** > **punktów końcowych**.

- **Na liście ACL dla punktu końcowego ruch SSH na komputerze wirtualnych docelowym.** ACL można określić dozwolone lub nie ruch przychodzący z Internetu, na podstawie jego adresu IP źródła. Niepoprawnie skonfigurowane ACL zapobiec ruch przychodzący SSH do punktu końcowego. Zapoznaj się z list ACL zapewnienie tego ruch przychodzący z publicznych adresów IP swojego serwera proxy lub innych serwera granicznego jest dozwolone. Aby uzyskać więcej informacji zobacz [informacje o dostęp do sieci control list (ACL)](../virtual-network/virtual-networks-acl.md).

Aby usunąć punkt końcowy jako źródło problemu, Usuń punkt końcowy bieżącego, utworzyć nowy punkt końcowy i określ nazwę SSH (port TCP 22 dla numeru portu publicznych i prywatnych). Aby uzyskać więcej informacji zobacz [Konfigurowanie punkty końcowe maszyny wirtualnej Azure](virtual-machines-windows-classic-setup-endpoints.md).

<a id="nsg"></a>
## <a name="source-4-network-security-groups"></a>Źródła 4: Grupy zabezpieczeń sieci

Grupy zabezpieczeń sieci umożliwiają mieć większą kontrolę nad dozwolonych ruch przychodzący i wychodzący. Możesz utworzyć reguły, które obejmować podsieci i usług w Azure wirtualną sieć w chmurze. Sprawdź reguły grupy zabezpieczeń sieciowych, tak aby upewnić się, że SSH ruchu do i z Internetu jest dozwolone.
Aby uzyskać więcej informacji zobacz [temat grup zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md).

## <a name="source-5-linux-based-azure-virtual-machine"></a>Źródło 5: Systemem Linux Azure maszyn wirtualnych

Ostatni Źródło potencjalnych problemów jest Azure samej maszyny wirtualnej.

![Diagram wyróżnienie systemem Linux Azure maszyn wirtualnych](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Jeśli nie zrobiono tego wcześniej, wykonaj instrukcje [o zresetowanie hasła lub SSH dla maszyn wirtualnych z systemem Linux](virtual-machines-linux-classic-reset-access.md).

Spróbuj ponownie nawiązać połączenie ze swojego komputera. Jeśli nadal nie, Oto kilka możliwych problemów:

- Usługa SSH nie jest uruchomiona na komputerze wirtualnych docelowym.
- Usługa SSH nie nasłuchują na TCP port 22. Aby to sprawdzić, instalowanie klienta telnet na komputerze lokalnym i uruchamianie "telnet *cloudServiceName*. cloudapp.net 22". To ustawienie określa, jeśli maszyny wirtualnej umożliwia komunikację przychodzący i wychodzący do punktu końcowego SSH.
- Lokalne zapory na komputerze wirtualnych docelowym ma reguły, które uniemożliwiają ruch SSH przychodzących i wychodzących.
- Wykrywanie intruzów lub monitorowania program działający na komputerze Azure wirtualnych sieci uniemożliwia SSH połączenia.


## <a name="additional-resources"></a>Dodatkowe zasoby
Aby uzyskać więcej informacji na temat rozwiązywania problemów z dostęp do aplikacji zobacz [Rozwiązywanie problemów z dostępem do uruchamiania aplikacji na Azure maszyn wirtualnych](virtual-machines-linux-troubleshoot-app-connection.md)