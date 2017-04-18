<properties
    pageTitle="Tworzenie maszyny uruchomiony MySQL | Microsoft Azure"
    description="Tworzenie Azure maszyn wirtualnych z systemem Windows Server 2012 R2 i bazy danych MySQL przy użyciu modelu Klasyczny wdrożenia."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>MySQL zostaną zainstalowane na komputerze wirtualnej utworzone za pomocą modelu Klasyczny wdrożenia systemem Windows Server 2012 R2

[MySQL](http://www.mysql.com) to popularne Otwórz źródło bazy danych SQL. Ten samouczek pokazano, jak zainstalować i uruchomić wersję społeczności MySQL 5.6.23 jako serwer MySQL na komputerze wirtualnych z systemem Windows Server 2012 R2. Aby uzyskać instrukcje dotyczące instalowania MySQL na Linux, odwołują się do: [jak zainstalować MySQL Azure](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>Tworzenie maszyny wirtualnej systemem Windows Server 2012 R2

Jeśli nie masz jeszcze maszyny z systemem Windows Server 2012 R2, można utworzyć maszyny wirtualnej za pomocą tego [samouczka](virtual-machines-windows-classic-tutorial.md) . 

## <a name="attach-a-data-disk"></a>Dołączanie dysku danych

Po utworzeniu maszyny wirtualnej można opcjonalnie dołączyć dysk dodatkowych danych. Jest to zalecane dla obciążenia produkcji oraz w celu uniknięcia brakiem miejsca na dysku OS (C:), który zawiera system operacyjny.

Zobacz, [jak dołączyć dysku danych maszyn wirtualnych systemu Windows](virtual-machines-windows-classic-attach-disk.md) i postępuj zgodnie z instrukcjami dotyczącymi dołączanie pusty dysk. Ustawienie **Brak** pamięci podręcznej hosta lub **tylko do odczytu**.

## <a name="log-on-to-the-virtual-machine"></a>Zaloguj się do maszyny wirtualnej

Następnie zostaną [logowania maszyn wirtualnych](virtual-machines-windows-classic-connect-logon.md) aby można było zainstalować MySQL.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Instalowanie i uruchamianie serwer MySQL społeczności na komputerze wirtualnych

Wykonaj poniższe czynności, aby zainstalować, konfigurować i uruchomić wersję społeczności serwer MySQL:

> [AZURE.NOTE] Te kroki dotyczą 5.6.23.0 wersji społeczności programu MySQL i Windows Server 2012 R2. Środowiska pracy użytkownika mogą się różnić w różnych wersjach usługi MySQL lub Windows Server.

1.  Po nawiązaniu połączenia maszyn wirtualnych za pomocą pulpitu zdalnego, kliknij pozycję **Programu Internet Explorer** z ekranu startowego.
2.  Wybierz przycisk **Narzędzia** w prawym górnym rogu (ikona koła cogged), a następnie kliknij polecenie **Opcje internetowe**. Kliknij kartę **Zabezpieczenia** , kliknij ikonę **Zaufane witryny** , a następnie kliknij przycisk **witryny** . Dodawanie http://*. mysql.com do listy zaufanych witryn. Kliknij pozycję * *Zamknij**, a następnie kliknij pozycję * *OK**.
3.  W pasku adresu internetowego Eksploratora, wpisz http://dev.mysql.com/downloads/mysql/.
4.  Aby znaleźć i pobrać najnowszą wersję pakietu Instalatora MySQL dla systemu Windows za pomocą witryny MySQL. Wybierając Instalatora MySQL, Pobierz w wersji zawierającej pełne pliku zestawu (na przykład mysql Instalatora — społeczności 5.6.23.0.msi o rozmiarze pliku 282.4 MB), a następnie zapisz Instalatora pakietu.
5.  Po zakończeniu Instalator pobierania, kliknij przycisk **Uruchom** , aby uruchom Instalatora.
6.  Na stronie **Umowa licencyjna** Zaakceptuj Umowę licencyjną i kliknij przycisk **Dalej**.
7.  Na stronie **Wybieranie typu konfiguracji** kliknij typ instalacji, który ma być, a następnie kliknij przycisk **Dalej**. Wybieranie typu ustawienia **tylko serwer** przyjęto założenie, następujące czynności.
8.  Na stronie **instalacji** kliknij polecenie **Wykonaj**. Po zakończeniu instalacji kliknij przycisk **Dalej**.
9.  Na stronie **Konfiguracja produktu** kliknij przycisk **Dalej**.
10. Na stronie **Typ i sieci** Określ usługi Konfiguracja docelowa łączności i wpisz opcje, takie jak TCP port w razie potrzeby. Wybierz pozycję **Pokaż opcje zaawansowane**, a następnie kliknij przycisk **Dalej**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. Na stronie **konta i role** Określ silne hasła root MySQL. Dodać kolejne konta użytkownika MySQL, stosownie do potrzeb, a następnie kliknij przycisk **Dalej**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. Na stronie **Usługi Windows** określić zmiany domyślnych ustawień uruchamiania serwer MySQL jako usługa Windows stosownie do potrzeb, a następnie kliknij przycisk **Dalej**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. Na stronie **Zaawansowane opcje** określić zmiany opcji rejestrowania, stosownie do potrzeb, a następnie kliknij przycisk **Dalej**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. Na stronie **Stosowanie konfiguracji serwera** kliknij polecenie **Wykonaj**. Po wykonaniu kroków konfiguracji, kliknij przycisk **Zakończ**.
15. Na stronie **Konfiguracja produktu** kliknij przycisk **Dalej**.
16. Na stronie **Zakończenia instalacji** kliknij pozycję **Dziennika Kopiuj do Schowka** , jeśli chcesz zbadać je później, a następnie kliknij przycisk **Zakończ**.
17. Na ekranie startowym wpisz **mysql**, a następnie kliknij **MySQL 5.6 wiersza polecenia klienta**.
18. Wprowadź hasło głównego określony w kroku 11 i zostanie wyświetlony z monitem miejsce, w którym można wysyłać polecenia, aby skonfigurować MySQL. Aby uzyskać szczegóły poleceń i składni zobacz [Instrukcje MySQL](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. Można także skonfigurować ustawienia domyślne konfiguracji serwera, takie jak katalogi base i danych i dyski, z wpisami w pliku 5.6\my-default.ini C:\Program Files (x86) \MySQL\MySQL serwera. Aby uzyskać więcej informacji, zobacz [5.1.2 ustawień domyślnych konfiguracji serwera](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Konfigurowanie punktów końcowych

Jeśli chcesz, aby usługa Serwer MySQL jest dostępna dla komputerów klienckich MySQL w Internecie, należy skonfigurować punkt końcowy dla port TCP, na którym oczekuje usługi Serwer MySQL i utworzyć dodatkowe reguły zapory systemu Windows. To TCP port 3306, o ile nie określono innego portu na stronie **Typ i sieci** (krok 10 poprzedniej procedury).


> [AZURE.NOTE] Należy więc rozważyć ryzyko związane z temu, ponieważ dzięki temu będzie usługi Serwer MySQL dostępne na wszystkich komputerach w Internecie. Można zdefiniować zestaw źródłowe adresy IP, które mogą korzystać z listy kontroli dostępu (ACL) punkt końcowy. Aby uzyskać więcej informacji zobacz [jak się punkty końcowe maszyn wirtualnych](virtual-machines-windows-classic-setup-endpoints.md).


Aby skonfigurować punktu końcowego usługi Serwer MySQL:

1.  W portalu klasyczny Azure kliknij **maszyn wirtualnych**, kliknij nazwę komputera wirtualnych MySQL, a następnie kliknij **punktów końcowych**.
2.  Na pasku poleceń kliknij przycisk **Dodaj**.
3.  Na stronie **Dodawanie punktu końcowego maszyn wirtualnych** kliknij strzałkę w prawo.
4.  Jeśli używasz domyślny port MySQL TCP 3306 kliknij **MySQL** w polu **Nazwa**, a następnie kliknij znacznik wyboru.
5.  Jeśli korzystasz z różnych portów TCP, wpisz unikatową nazwę w polu **Nazwa**. Wybierz protokół **TCP** , wpisz numer portu w **Zarówno publicznej Port** i **Prywatny**, a następnie kliknij znacznik wyboru.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Dodawanie reguły zapory systemu Windows w celu zezwalanie na ruch MySQL

Aby dodać regułę zapory systemu Windows, która umożliwia MySQL ruchu z Internetu, uruchom następujące polecenie w wierszu polecenia z podwyższonym poziomem uprawnień programu Windows PowerShell na serwerze wirtualnej MySQL.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>Testowanie połączenia zdalnego


Aby przetestować połączenia zdalnego z usługą Serwer MySQL uruchomionych Azure maszyny wirtualnej, należy najpierw określić nazwę DNS odpowiadający usługi w chmurze, zawierający maszyny wirtualnej uruchomiony serwer MySQL.

1.  W portalu klasyczny Azure kliknij **maszyn wirtualnych**, kliknij nazwę komputera wirtualnych serwer MySQL, a następnie kliknij **pulpitu nawigacyjnego**.
2.  Na pulpicie nawigacyjnym maszyn wirtualnych Uwaga wartości z **Pola Nazwa DNS** w sekcji **Szybkie skrócie** . Oto przykład:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  Na komputerze lokalnym systemem MySQL lub klienta MySQL uruchom następujące polecenie, aby zalogować się jako użytkownik MySQL.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    Na przykład dbadmin3 nazwa użytkownika MySQL, a nazwa DNS testmysql.cloudapp.net maszyny wirtualnej, użyj następującego polecenia.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o uruchamianiu MySQL, zapoznaj się z [Dokumentacją MySQL](http://dev.mysql.com/doc/).
