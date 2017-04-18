<properties
    pageTitle="Konfigurowanie zawsze na grupy dostępności w maszyn wirtualnych Azure — klasyczny"
    description="Utwórz grupę zawsze włączone dostępność Azure maszyn wirtualnych. Przede wszystkim samouczku interfejsu użytkownika i narzędzia zamiast skryptów."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm---classic"></a>Konfigurowanie zawsze na grupy dostępności w maszyn wirtualnych Azure — klasyczny

> [AZURE.SELECTOR]
- [Menedżer zasobów: szablonu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Menedżer zasobów: ręczne](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasyczny: interfejs użytkownika](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasyczny: programu PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Ten samouczek w końcu do końca pokazano, jak za pomocą programu SQL Server zawsze na uruchomionych Azure maszyn wirtualnych grupy dostępności.

Na koniec samouczka rozwiązanie programu SQL Server zawsze na Azure będzie się składał z następujących elementów:

- Wirtualna sieć zawierająca wiele podsieci, łącznie z zewnętrzną i podsieć wewnętrznej

- Kontrolera domeny z domeną usługi Active Directory (AD)

- Dwa SQL Server maszyny wirtualne wdrożyć podsieci wewnętrznej i dołączony do domeny AD

- Klaster WSFC węzeł 3 z modelem kworum Większość węzłów

- Grupy dostępności z dwoma replikami synchroniczne Zatwierdź dostępność bazy danych

Na poniższym rysunku jest graficzna reprezentacja rozwiązanie.

![Testowanie architektura ćwiczenia AG platformy Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Należy zauważyć, że jest jedną z możliwych konfiguracji. Na przykład można zminimalizować liczbę maszyny wirtualne dla grupy dostępność replice dwóch w celu zapisania na godziny obliczeń w Azure przy użyciu kontrolera domeny jako monitora udostępniania plików kworum w klastrze WSFC węzeł 2. Ta metoda zmniejsza statystykę maszyn wirtualnych przez jeden z podanych konfiguracji.

Tego samouczka przyjęto założenie następujące czynności:

- Masz już konto Azure.

- Wiesz już, jak inicjować obsługę klasycznego programu SQL Server maszyn wirtualnych z galerii maszyn wirtualnych przy użyciu graficznego interfejsu użytkownika.

- Masz już pełne zrozumienie istoty zawsze na grupy dostępności. Aby uzyskać więcej informacji zobacz [Zawsze na grupy dostępności (program SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Jeśli interesuje Cię zawsze na grupy dostępności za pomocą programu SharePoint, zobacz też [Konfigurowanie programu SQL Server 2012 zawsze na dostępność grup programu SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Tworzenie wirtualnych sieci i serwer kontrolera domeny

Rozpoczynać nowe konto Azure wersji próbnej. Po zakończeniu konfiguracji konta, należy na ekranie głównym portalu klasyczny Azure.

1. Kliknij przycisk **Nowy** w lewym dolnym rogu strony, tak jak pokazano poniżej.

    ![Kliknij przycisk Nowy w portalu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)

1. Kliknij pozycję **Usługi sieciowe**, a następnie kliknij **Wirtualnej sieci** , a następnie kliknij, **Tworzyć niestandardowe**, tak jak pokazano poniżej.

    ![Tworzenie wirtualnych sieci](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)

1. W oknie dialogowym **Tworzenie wirtualnych sieci** Utwórz nowy wirtualnej sieci, krokowe stron z poniższych ustawień. 

  	|Strony|Ustawienia|
|---|---|
|Szczegóły wirtualnej sieci|**Nazwa = ContosoNET**<br/>**REGION = US zachód**|
|Serwery DNS i połączenia VPN|Brak|
|Obszar adresów wirtualnych sieci|Ustawienia są wyświetlane poniżej ekranu: ![Tworzenie wirtualnych sieci](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png)|

1. Następnie możesz utworzyć maszyn wirtualnych, będzie używany jako domeny kontrolera. Kliknij ponownie **Nowy** , a następnie **obliczenia**, a następnie **maszyn wirtualnych**, a następnie **Z galerii**tak jak pokazano poniżej.

    ![Tworzenie maszyn wirtualnych](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)

1. W oknie dialogowym **Tworzenie maszyn wirtualnych** należy skonfigurować nowe maszyn wirtualnych przy krokowe stron z poniższych ustawień. 

  	|Strony|Ustawienia|
|---|---|
|Wybór maszyn wirtualnych systemu operacyjnego|Windows Server 2012 R2 w centrum danych|
|Konfiguracja maszyn wirtualnych|**Data wydania wersji** = (Najnowsza wersja)<br/>**Nazwa maszyn wirtualnych** = ContosoDC<br/>**Warstwa** = standardowe<br/>**Rozmiar** = A2 (rdzenie 2)<br/>**Nową nazwę użytkownika** = AzureAdmin<br/>**Nowe hasło** = Contoso! 000<br/>**POTWIERDŹ** = Contoso! 000|
|Konfiguracja maszyn wirtualnych|**Usługa w CHMURZE** = utworzenie nowej usługi w chmurze<br/>**Nazwa DNS usługi CLOUD** = nazwę usługi cloud unikatowe<br/>**Nazwa DNS** = unikatową nazwę (ex: ContosoDC123)<br/>**REGION/grupy KOLIGACJI/wirtualnych sieci** = ContosoNET<br/>**Wirtualna PODSIECI** = Back(10.10.2.0/24)<br/>**Konto miejsca do magazynowania** = Użyj konta automatycznie wygenerowanego miejsca do magazynowania<br/>**Ustawianie dostępności** = (Brak)|
|Opcje maszyn wirtualnych|Użyj wartości domyślnych|

Po skonfigurowaniu nowego maszyn wirtualnych poczekaj maszyn wirtualnych za provsioned. Zajmie trochę czasu, aby ukończyć, a użytkownik kliknie na karcie **maszyn wirtualnych** w portalu klasyczny Azure, można zobaczyć stanów następuje ContosoDC z **początkowej (obsługi)** do **zatrzymania**, **Uruchamianie**, **uruchomiony (obsługi)**i na końcu **rozpocząć pracę**.

Serwer kontrolera domeny jest teraz pomyślnie zainicjowano obsługę administracyjną. Następnie skonfiguruj usługi Active Directory na tym serwerze kontrolera domeny.

## <a name="configure-the-domain-controller"></a>Konfigurowanie kontrolera domeny

W następującej procedurze skonfigurowaniu komputera ContosoDC jako kontrolera domeny dla corp.contoso.com.

1. W portalu wybierz komputer **ContosoDC** . Na karcie **pulpitu nawigacyjnego** kliknij przycisk **Połącz** , aby otworzyć plik RDP dostęp zdalny do pulpitu.

    ![Nawiązywanie połączenia z komputera Vritual](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)

1. Zaloguj się przy użyciu swojego skonfigurowanego konta (**\AzureAdmin**) i hasła administratora (**Contoso! 000**).

1. Domyślnie powinny być wyświetlane na pulpicie nawigacyjnym **Menedżer serwera** .

1. Kliknij łącze **Dodaj role i funkcje** na pulpicie nawigacyjnym.

    ![Eksplorator serwera Dodawanie ról](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)

1. Wybierz przycisk **Dalej** , aż przejdziesz do sekcji **Role serwerów** .

1. Wybierz pozycję ról w **Usługach domenowych Active Directory** i **Serwera DNS** . Gdy zostanie wyświetlony monit, Dodaj wszelkie dodatkowe funkcje wymagane przez te role.

    >[AZURE.NOTE] Zostanie wyświetlony przy sprawdzaniu poprawności ostrzeżenie, że jest żaden adres IP. Jeśli testujesz konfiguracji, kliknij przycisk Kontynuuj. Produkcji scenariusze, [za pomocą programu PowerShell, aby ustawić statyczny adres IP komputera kontrolera domeny](./virtual-network/virtual-networks-reserved-private-ip.md).

    ![Okno dialogowe role Dodawanie](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)

1. Kliknij przycisk **Dalej** , aż zostanie wyświetlona sekcja **potwierdzenia** . Zaznacz pole wyboru **automatycznie, jeśli jest to wymagane, uruchom ponownie serwer docelowy** .

1. Kliknij przycisk **Zainstaluj**.

1. Po funkcji ukończyć instalacji, wróć do pulpitu nawigacyjnego **Menedżer serwera** .

1. Wybierz opcję nowych **usług AD DS** w okienku po lewej stronie.

1. Kliknij łącze **więcej** na żółtym pasku ostrzeżenie.

    ![AD DS okno na maszyn wirtualnych serwera DNS](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)

1. W kolumnie **Akcja** okna dialogowego **Wszystkie szczegóły zadania serwera** kliknij pozycję **Podwyższ poziom ten serwer do kontrolera domeny**.

1. W oknie dialogowym **Kreator konfiguracji usług domenowych Active Directory**za pomocą następujących wartości:

  	|Strony|Ustawienie|
|---|---|
|Konfiguracja wdrożenia|**Dodaj nowy las** = wybrane<br/>**Nazwa domeny głównej** = corp.contoso.com|
|Opcje kontrolera domeny|**Hasło** = Contoso! 000<br/>**Potwierdź hasło** = Contoso! 000|

1. Kliknij przycisk **Dalej** przez inne strony w kreatorze. Na stronie **Sprawdź wymagania wstępne dotyczące** upewnij się, zostanie wyświetlony następujący komunikat: **wszystkie testy wstępne przekazywane pomyślnie**. Należy zauważyć, że należy przejrzeć wszelkie stosowne ostrzeżenia, ale istnieje możliwość kontynuować instalację.

1. Kliknij przycisk **Zainstaluj**. Maszyny wirtualnej **ContosoDC** zostanie automatycznie ponownie uruchomiony.

## <a name="configure-domain-accounts"></a>Konfigurowanie konta domeny

Następne kroki konfigurowania konta usługi Active Directory (AD) do późniejszego użycia.

1. Zaloguj się do komputera **ContosoDC** .

1. W **Menedżerze serwera** wybierz pozycję **Narzędzia** , a następnie kliknij pozycję **Centrum administracyjnego usługi Active Directory**.

    ![Centrum administracyjne usługi Active Directory](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)

1. W **Centrum administracyjnym usługi Active Directory** wybierz **corp (lokalny)** w lewym okienku.

1. W prawym okienku **zadań** wybierz pozycję **Nowy** , a następnie kliknij **użytkownika**. Użyj następujących ustawień:

  	|Ustawienie|Wartość|
|---|---|
|**Imię**|Instalowanie|
|**Atrybuty SamAccountName użytkownika**|Instalowanie|
|**Hasło**|Contoso! 000|
|**Potwierdź hasło**|Contoso! 000|
|**Inne opcje dotyczące haseł**|Zaznaczone|
|**Hasło nigdy nie wygasa**|Zaznaczone pole wyboru|

1. Kliknij **przycisk OK** , aby utworzyć **Instalowanie** użytkownika. To konto będzie używane do konfigurowania klaster pracy awaryjnej i grupy dostępności.

1. Tworzenie dwóch dodatkowych użytkowników z te same kroki: **CORP\SQLSvc1** i **CORP\SQLSvc2**. Te konta będą używane dla wystąpienia programu SQL Server. Następnie należy udzielić **CORP\Install** uprawnienia do konfigurowania systemu Windows usługa pracy awaryjnej klaster (WSFC).

1. W **Centrum administracyjnym usługi Active Directory**wybierz pozycję **corp (lokalny)** w lewym okienku. Następnie w okienku **zadań** po prawej stronie kliknij pozycję **Właściwości**.

    ![Właściwości CORP użytkownika](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)

1. Wybierz **rozszerzenia**, a następnie kliknij przycisk **Zaawansowane** na karcie **Zabezpieczenia** .

1. W oknie dialogowym **Zaawansowane ustawienia zabezpieczeń dla corp** . Kliknij przycisk **Dodaj**.

1. Kliknij pozycję **Wybierz podmiot zabezpieczeń**. Następnie wyszukaj **CORP\Install**. Kliknij **przycisk OK**.

1. Wybierz uprawnienia do **odczytu wszystkich właściwości** i **obiektów Tworzenie komputera** .

    ![Uprawnienia użytkowników Corp](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)

1. Kliknij **przycisk OK**, a następnie kliknij przycisk **OK** . Zamknij okno właściwości corp.

Teraz, gdy skończysz konfigurowania usługi Active Directory i obiektów użytkownika, zostanie Utwórz trzy maszyny wirtualne serwera SQL i połączyć je do tej domeny.

## <a name="create-the-sql-server-vms"></a>Tworzenie maszyny wirtualne serwera SQL

Następnie utwórz trzy pośrednictwem SMS, w tym węzła WSFC i dwóch maszyny wirtualne serwera SQL. Aby utworzyć każdej maszyny wirtualne, wróć do portalu klasyczny Azure kliknij pozycję **Nowy**, **obliczenia**, **maszyn wirtualnych**, a następnie **Z galerii**. Ułatwia tworzenie maszyny wirtualne używać szablonów w poniższej tabeli.

|Strony|VM1|VM2|VM3|
|---|---|---|---|
|Wybór maszyn wirtualnych systemu operacyjnego|**Windows Server 2012 R2 w centrum danych**|**SQL Server 2014 RTM Enterprise**|**SQL Server 2014 RTM Enterprise**|
|Konfiguracja maszyn wirtualnych|**Data wydania wersji** = (Najnowsza wersja)<br/>**Nazwa maszyn wirtualnych** = ContosoWSFCNode<br/>**Warstwa** = standardowe<br/>**Rozmiar** = A2 (rdzenie 2)<br/>**Nową nazwę użytkownika** = AzureAdmin<br/>**Nowe hasło** = Contoso! 000<br/>**POTWIERDŹ** = Contoso! 000|**Data wydania wersji** = (Najnowsza wersja)<br/>**Nazwa maszyn wirtualnych** = ContosoSQL1<br/>**Warstwa** = standardowe<br/>**Rozmiar** = A3 (rdzenie 4)<br/>**Nową nazwę użytkownika** = AzureAdmin<br/>**Nowe hasło** = Contoso! 000<br/>**POTWIERDŹ** = Contoso! 000|**Data wydania wersji** = (Najnowsza wersja)<br/>**Nazwa maszyn wirtualnych** = ContosoSQL2<br/>**Warstwa** = standardowe<br/>**Rozmiar** = A3 (rdzenie 4)<br/>**Nową nazwę użytkownika** = AzureAdmin<br/>**Nowe hasło** = Contoso! 000<br/>**POTWIERDŹ** = Contoso! 000|
|Konfiguracja maszyn wirtualnych|**Usługa w CHMURZE** = poprzednio utworzone unikatowe chmury usługi DNS nazwa (ex: ContosoDC123)<br/>**REGION/grupy KOLIGACJI/wirtualnych sieci** = ContosoNET<br/>**Wirtualna PODSIECI** = Back(10.10.2.0/24)<br/>**Konto miejsca do magazynowania** = Użyj konta automatycznie wygenerowanego miejsca do magazynowania<br/>**Ustawianie dostępności** = Utwórz dostępność Ustawianie<br/>**Nazwa zestawu dostępność** = SQLHADR|**Usługa w CHMURZE** = poprzednio utworzone unikatowe chmury usługi DNS nazwa (ex: ContosoDC123)<br/>**REGION/grupy KOLIGACJI/wirtualnych sieci** = ContosoNET<br/>**Wirtualna PODSIECI** = Back(10.10.2.0/24)<br/>**Konto miejsca do magazynowania** = Użyj konta automatycznie wygenerowanego miejsca do magazynowania<br/>**Ustawianie dostępności** = SQLHADR (można również skonfigurować dostępność po utworzeniu komputera. Wszystkie trzy komputery powinny być przypisane do zestawu dostępność SQLHADR.)|**Usługa w CHMURZE** = poprzednio utworzone unikatowe chmury usługi DNS nazwa (ex: ContosoDC123)<br/>**REGION/grupy KOLIGACJI/wirtualnych sieci** = ContosoNET<br/>**Wirtualna PODSIECI** = Back(10.10.2.0/24)<br/>**Konto miejsca do magazynowania** = Użyj konta automatycznie wygenerowanego miejsca do magazynowania<br/>**Ustawianie dostępności** = SQLHADR (można również skonfigurować dostępność po utworzeniu komputera. Wszystkie trzy komputery powinny być przypisane do zestawu dostępność SQLHADR.)|
|Opcje maszyn wirtualnych|Użyj wartości domyślnych|Użyj wartości domyślnych|Użyj wartości domyślnych|

<br/>

>[AZURE.NOTE] Poprzednią konfigurację sugerowanie maszyn wirtualnych STANDARDOWEJ warstwy, ponieważ maszyn podstawowe warstwa równoważenia obciążenia punkty końcowe wymagane później utworzyć detektory grupy dostępności nie są obsługiwane. Ponadto rozmiarów maszynowego sugerowane tutaj są przeznaczone dla badań grupy dostępności maszyny wirtualne Azure. Celu uzyskania najlepszej wydajności na obciążenia produkcji zobacz zalecenia dotyczące rozmiarów komputera programu SQL Server i konfiguracji [wydajności najważniejsze wskazówki dotyczące programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-performance.md).

Po trzech maszyny wirtualne są w pełni obsługi administracyjnej, należy połączyć je z domeną **corp.contoso.com** i udzielanie uprawnień administracyjnych CORP\Install maszyny. Aby to zrobić, wykonaj następujące czynności dla każdej z trzech maszyny wirtualne.

1. Najpierw zmienić preferowany adres serwera DNS. Zacznij od pobierania poszczególnych maszyn wirtualnych (RDP) pliku z pulpitu zdalnego w katalogu lokalnym, zaznaczając maszyn wirtualnych na liście i klikając przycisk **Połącz** . Aby zaznaczyć maszyny, kliknij dowolne miejsce ale pierwszą komórkę w wierszu, jak pokazano poniżej.

    ![Pobierz plik RDP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)

1. Uruchom pobrany plik RDP i zalogować się do maszyn wirtualnych, za pomocą usługi skonfigurowanego konta (**BUILTIN\AzureAdmin**) i hasła administratora (**Contoso! 000**).

1. Gdy logujesz się powinna być widoczna na pulpicie nawigacyjnym **Menedżer serwera** . W lewym okienku kliknij pozycję **Lokalny serwer** .

1. Wybierz łącze **przypisany adres IP protokołu IPv4 przez DHCP, włączony protokół IPv6** .

1. W oknie **Połączenia** wybierz ikonę Sieć.

    ![Zmienianie serwera DNS preferowanej maszyn wirtualnych](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)

1. Na pasku poleceń kliknij przycisk **Zmień ustawienia tego połączenia** (w zależności od rozmiaru okna programu, może być konieczne kliknij Podwójna strzałka w prawo, aby wyświetlić to polecenie).

1. Wybierz **Protokół internetowy w wersji 4 (TCP/IPv4)** , a następnie kliknij polecenie Właściwości.

1. Wybierz pozycję Użyj następującego serwera DNS adresów i określić **10.10.2.4** w **polu Preferowany serwer DNS**.

1. Adres **10.10.2.4** to adres przypisany do maszyn wirtualnych w podsieci 10.10.2.0/24 w Azure wirtualnej sieci, a tego maszyn wirtualnych jest **ContosoDC**. Aby sprawdzić adres IP **ContosoDC**, użyj **narzędzia nslookup contosodc** w wierszu polecenia, tak jak pokazano poniżej.

    ![Znajdowanie adresu IP dla kontrolera domeny przy użyciu narzędzia NSLOOKUP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)

1. Kliknij przycisk O**K** , a następnie **Zamknij** aby zatwierdzić zmiany. Teraz możesz mogą dołączyć do maszyn wirtualnych do **corp.contoso.com**.

1. W oknie **Lokalnego serwera** kliknij łącze **Grupa robocza** .

1. W sekcji **Nazwa komputera** kliknij przycisk **Zmień**.

1. Zaznacz pole wyboru **domeny** i w polu tekstowym wpisz **corp.contoso.com** . Kliknij **przycisk OK**.

1. W oknie dialogowym podręcznego **Zabezpieczeń systemu Windows** , określ poświadczenia dla domyślnego konta administratora domeny (**CORP\AzureAdmin**) i hasło (**Contoso! 000**).

1. Gdy zostanie wyświetlony komunikat "Witamy domeny corp.contoso.com", kliknij **przycisk OK**.

1. Kliknij przycisk **Zamknij**, a następnie kliknij przycisk **Uruchom ponownie** w oknie podręcznym.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-vm"></a>Dodaj użytkownika Corp\Install na poszczególnych maszyn wirtualnych jako administrator:

1. Poczekaj, aż uruchomieniu maszyn wirtualnych, a następnie uruchom plik RDP ponownie, aby zalogować się do maszyn wirtualnych, przy użyciu konta **BUILTIN\AzureAdmin** .

1. W **Menedżerze serwera** wybierz pozycję **Narzędzia**, a następnie kliknij **Zarządzanie komputerem**.

    ![Zarządzanie komputerem](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)

1. W oknie **Zarządzanie komputerem** rozwiń **Użytkownicy i grupy lokalne**, a następnie wybierz pozycję **grupy**.

1. Kliknij dwukrotnie grupy **administratorów** .

1. W oknie dialogowym **Właściwości: Administratorzy** kliknij przycisk **Dodaj** .

1. Wprowadź użytkownika **CORP\Install**, a następnie kliknij **przycisk OK**. Gdy zostanie wyświetlony monit o poświadczenia, za pomocą konta **AzureAdmin** z **Contoso! 000** hasła.

1. Kliknij **przycisk OK** , aby zamknąć okno dialogowe **Właściwości administratora** .

### <a name="add-the-failover-clustering-feature-to-each-vm"></a>Dodawanie funkcji **Awaryjnej** do każdego maszyn wirtualnych.

1. Na pulpicie nawigacyjnym **Menedżera serwera** kliknij pozycję **Dodaj role i funkcje**.

1. W **Kreatorze funkcje i Dodaj role**kliknij przycisk **Dalej** , aż przejdziesz na stronę **Funkcje** .

1. Wybierz pozycję **awaryjnej**. Gdy zostanie wyświetlony monit, Dodaj inne funkcje zależne.

    ![Dodawanie funkcji awaryjnej do maszyn wirtualnych](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)

1. Kliknij przycisk **Dalej**, a następnie kliknij przycisk **Zainstaluj** na stronie **potwierdzenia** .

1. Po zakończeniu instalacji funkcji **Awaryjnej** , kliknij przycisk **Zamknij**.

1. Wyloguj się z maszyn wirtualnych.

1. Powtórz czynności opisane w tej sekcji dla wszystkich trzech serwerów — **ContosoWSFCNode**, **ContosoSQL1**i **ContosoSQL2**.

Teraz zainicjowano pośrednictwem SQL Server SMS i uruchomiony, ale są instalowane z programem SQL Server przy użyciu opcji domyślnych.

## <a name="create-the-wsfc-cluster"></a>Utwórz klaster WSFC

W tej sekcji możesz utworzyć klaster WSFC, w którym będzie przechowywana grupy dostępności, które będą później tworzone. Już należy wykonać następujące czynności, aby każdej z trzech maszyny wirtualne użyje w klastrze WSFC:

- W pełni obsługi administracyjnej platformy Azure

- Sprzężone maszyn wirtualnych do domeny

- Dodano **CORP\Install** do lokalnej grupy Administratorzy

- Dodać funkcję awaryjnej

Wymagania wstępne dotyczące na poszczególnych maszyn wirtualnych all są przed dołączeniem go do klastrów WSFC.

Należy również zauważyć, że Azure wirtualną sieć nie działają w taki sam sposób jak sieci lokalnej. Musisz utworzyć klaster w następującej kolejności:

1. Utwórz klaster pojedynczy węzeł w jednym z węzłów (**ContosoSQL1**).

1. Modyfikowanie klaster adresu IP w celu nieużywany adres IP (**10.10.2.101**).

1. Wyświetl nazwę klaster w trybie online.

1. Dodaj inne węzły (**ContosoSQL2** i **ContosoWSFCNode**).

Wykonaj poniższe czynności, aby wykonać te zadania, które pełni konfiguruje klaster.

1. Uruchamianie pliku RDP **ContosoSQL1** i zaloguj się przy użyciu konta domeny **CORP\Install**.

1. Na pulpicie nawigacyjnym **Menedżer serwera** wybierz pozycję **Narzędzia**, a następnie kliknij **Menedżer klastrów pracy awaryjnej**.

1. W okienku po lewej stronie kliknij prawym przyciskiem myszy **Menedżer klastrów pracy awaryjnej**, a następnie kliknij przycisk **Utwórz klaster**, tak jak pokazano poniżej.

    ![Utwórz klaster](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)

1. W kreatorze Tworzenie klaster tworzenie klastrze jeden węzeł krokowe stron z poniższych ustawień:

  	|Strony|Ustawienia|
|---|---|
|Przed rozpoczęciem|Użyj wartości domyślnych|
|Zaznacz opcję serwery|Wpisz **ContosoSQL1** w polu **Nazwa serwera wprowadź** , a następnie kliknij przycisk **Dodaj**|
|Ostrzeżenie dotyczące sprawdzania poprawności|Wybierz pozycję **nr nie wymagają pomocy technicznej firmy Microsoft dla tego klaster i dlatego nie należy przeprowadzić testy poprawności. Po kliknięciu przycisku dalej kontynuować tworzenie klaster**.|
|Punkt dostępu do administrowania klastrem|Typ **Cluster1** w **nazwie klaster**|
|Potwierdzenia|Użyj ustawień domyślnych, chyba że używasz spacje miejsca do magazynowania. Zobacz uwagi poniżej tej tabeli.|

    >[AZURE.WARNING] Jeśli korzystasz z [Spacje miejsca do magazynowania](https://technet.microsoft.com/library/hh831739), który grupuje wiele dysków w puli miejsca do magazynowania, możesz wyczyść pole wyboru **Dodaj wszystkie miejsca do magazynowania z klastrem kwalifikuje się** na stronie **potwierdzenia** . Jeśli nie zaznaczenie tej opcji, dyski wirtualne zostanie odłączony podczas procesu klastrów. W wyniku również nie będzie widoczna w Menedżera dysków lub Eksploratora aż spacje miejsca do magazynowania zostaną usunięte z klaster i ponownie nałożona przy użyciu programu PowerShell.

1. W lewym okienku rozwiń **Menedżer klastrów pracy awaryjnej**, a następnie kliknij pozycję **Cluster1.corp.contoso.com**.

1. W środkowym okienku przewiń w dół do sekcji **Zasobów podstawowych klaster** , a następnie rozwiń listę **Nazwa: Clutser1** szczegóły. Powinna być widoczna **Nazwa** i zasoby **Adres IP** w stanie **nie powiodło się** . Zasób adresu IP nie można przełączyć online, ponieważ klaster przypisano ten sam adres IP, jak na komputerze, która jest zduplikowany adres.

1. Kliknij prawym przyciskiem myszy awarii zasobu **Adres IP** , a następnie kliknij polecenie **Właściwości**.

    ![Klaster Właściwości](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)

1. Zaznacz **Statyczny adres IP** i określ **10.10.2.101** w polu tekstowym adres. Następnie kliknij **przycisk OK**.

1. W sekcji **Zasobów podstawowych klaster** , kliknij prawym przyciskiem myszy **Nazwa: Cluster1** i kliknij przycisk **Przejdź do trybu Online**. Następnie zaczekaj, aż oba zasoby są w trybie online. Nazwa zasobu Klaster przechodzi do trybu online, jest aktualizowana na serwerze kontrolera domeny przy użyciu nowego konta komputera AD. To konto AD będą używane do uruchamiania usługi grupy grupowany dostępność później.

1. Ponadto możesz dodać pozostałe węzły z klastrem. W drzewie przeglądarki kliknij prawym przyciskiem myszy **Cluster1.corp.contoso.com** , a następnie kliknij przycisk **Dodaj węzeł**, tak jak pokazano poniżej.

    ![Dodawanie węzła z klastrem](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)

1. W oknie dialogowym **Kreatora dodawania węzła**kliknij przycisk **Dalej**. Na stronie **Wybierz serwery** Dodawanie **ContosoSQL2** i **ContosoWSFCNode** do listy wpisywanie w polu **Nazwa serwera wprowadź** nazwę serwera, a następnie klikając polecenie **Dodaj**. Gdy skończysz, kliknij przycisk **Dalej**.

1. Na stronie **Sprawdzanie poprawności ostrzeżenie** , kliknij przycisk **nie** (w środowisku produkcyjnym należy przeprowadzić testy poprawności). Następnie kliknij przycisk **Dalej**.

1. Na stronie **potwierdzenia** kliknij przycisk **Dalej** do węzły należy dodać.

    >[AZURE.WARNING] Jeśli korzystasz z [Spacje miejsca do magazynowania](https://technet.microsoft.com/library/hh831739), który grupuje wiele dysków w puli miejsca do magazynowania, musi wyczyść pole wyboru **Dodaj wszystkich odpowiedniej miejsca do magazynowania z klastrem** . Jeśli nie zaznaczenie tej opcji, dyski wirtualne zostanie odłączony podczas procesu klastrów. W wyniku również nie będzie widoczna w Menedżera dysków lub Eksploratora aż spacje miejsca do magazynowania zostaną usunięte z klaster i ponownie nałożona przy użyciu programu PowerShell.

1. Gdy węzły są dodawane do klaster, kliknij przycisk **Zakończ**. Menedżer klastrów pracy awaryjnej powinna zostać wyświetlona, że klaster zawiera trzy węzły i był widoczny w kontenerze **węzły** .

1. Wyloguj się z sesji pulpitu zdalnego.

## <a name="prepare-the-sql-server-instances-for-availability-group"></a>Przygotowywanie wystąpienia programu SQL Server do grupy dostępności

W tej sekcji będą wykonaj następujące czynności na **ContosoSQL1** i **contosoSQL2**:

- Dodawanie identyfikatora logowania dla **NT\SYSTEM** z niezbędnych uprawnień, Ustaw domyślne wystąpienie programu SQL Server

- Dodawanie **CORP\Install** rolę administratora systemu do domyślnego wystąpienia programu SQL Server

- Otwórz zaporę dla dostępu zdalnego programu SQL Server

- Włączanie funkcji zawsze na grupy dostępności

- Zmienianie konta usługi programu SQL Server odpowiednio **CORP\SQLSvc1** i **CORP\SQLSvc2**

Te akcje mogą być wykonywane w dowolnej kolejności. Jednak poniższe kroki prowadzi zgodnie z ich kolejnością. Postępuj zgodnie z instrukcjami dla **ContosoSQL1** i **ContosoSQL2**:

1. Jeśli użytkownik nie zalogował się z sesji pulpitu zdalnego dla maszyn wirtualnych, zrób to teraz.

1. Uruchamianie plików RDP **ContosoSQL1** i **ContosoSQL2** i zaloguj się jako **BUILTIN\AzureAdmin**.

1. Najpierw dodaj **NT\SYSTEM** logowania do programu SQL Server i z odpowiednimi uprawnieniami. Uruchom program **SQL Server Management Studio**.

1. Kliknij przycisk **Połącz** , aby nawiązać połączenie z domyślnym wystąpieniem programu SQL Server.

1. W **Eksploratorze obiektów**rozwiń **zabezpieczeń**, a następnie rozwiń węzeł **logowania**.

1. Kliknij prawym przyciskiem myszy **NT\SYSTEM** logowania, a następnie kliknij polecenie **Właściwości**.

1. Na stronie **zabezpieczanych obiektów** dla lokalnego serwera wybierz pozycję **Udziel** dla następujących uprawnień, a następnie kliknij **przycisk OK**.

    - Zmienić wszystkie grupy dostępności

    - Łączenie SQL

    - Wyświetlanie stanu serwera

1. Następnie dodaj **CORP\Install** rolę **administratora systemu** domyślne wystąpienie programu SQL Server. W **Eksploratorze obiektów**kliknij prawym przyciskiem myszy **logowania** , a następnie kliknij przycisk **Nowe logowanie**.

1. Wpisz **CORP\Install** w polu **Nazwa logowania**.

1. Na stronie **Role serwerów** wybierz pozycję **grupy**. Następnie kliknij **przycisk OK**. Po utworzeniu logowania będzie ono widoczne po rozwinięciu **logowania** w **Eksploratorze obiektów**.

1. Następnie możesz utworzyć reguły zapory dla programu SQL Server. Na ekranie **startowym** Uruchom **Zapora systemu Windows z zabezpieczeniami zaawansowanymi**.

1. W okienku po lewej stronie wybierz **Reguły przychodzące**. W okienku po prawej stronie kliknij polecenie **Nowa reguła**.

1. Na stronie **Typ reguły** wybierz pozycję **Program**, a następnie kliknij przycisk **Dalej**.

1. Na stronie **programu** zaznacz **Ta ścieżka programu** i wpisz %ProgramFiles%\Microsoft **SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** w polu tekstowym (jeśli jest wykonanie tych instrukcji, ale przy użyciu programu SQL Server 2012, katalogu programu SQL Server jest **MSSQL11. MSSQLSERVER**). Następnie kliknij przycisk **Dalej**.

1. Na stronie **Akcja** zachować **Zezwalaj na połączenie** zaznaczone, a następnie kliknij przycisk **Dalej**.

1. Na stronie **profilu** Zaakceptuj ustawienia domyślne, a następnie kliknij przycisk **Dalej**.

1. Na stronie **Nazwa** Określ nazwę reguły, na przykład **Programu SQL Server (reguły programu)** w polu tekstowym **Nazwa** , a następnie kliknij przycisk **Zakończ**.

1. Następnie należy włączyć funkcję **Zawsze na grupy dostępności** . Na ekranie **startowym** Uruchom **Menedżer konfiguracji programu SQL Server**.

1. W drzewie przeglądarki kliknij **Usług SQL Server**, a następnie kliknij prawym przyciskiem myszy usługę **Programu SQL Server (MSSQLSERVER)** , a następnie kliknij polecenie **Właściwości**.

1. Kliknij kartę **Zawsze na wysokiej dostępności** , a następnie wybierz pozycję **Włącz zawsze na dostępność grupy**, tak jak pokazano poniżej, a następnie kliknij przycisk **Zastosuj**. Kliknij **przycisk OK** w oknie podręcznym, a nie zamykaj okna właściwości jeszcze. Po zmianie konta usługi zostaną ponownie uruchomić usługę programu SQL Server.

    ![Zawsze włączyć grupy dostępności](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)

1. Następnie możesz zmienić konto usługi programu SQL Server. Kliknij kartę **Logowania** , a następnie wpisz **CORP\SQLSvc1** (w przypadku **ContosoSQL1**) lub **CORP\SQLSvc2** (w przypadku **ContosoSQL2**) w polu **Nazwa konta**, a następnie wprowadź i Potwierdź hasło, a następnie kliknij **przycisk OK**.

1. W oknie podręcznym kliknij przycisk **Tak,** Aby ponownie uruchomić usługę programu SQL Server. Po uruchomieniu usługi SQL Server, zmiany wprowadzone w oknie dialogowym właściwości są skuteczne.

1. Wyloguj się z maszyny wirtualne.

## <a name="create-the-availability-group"></a>Tworzenie grupy dostępności

Teraz możesz przystąpić do konfigurowania grupy dostępności. Oto konspektu będzie zrobić:

- Tworzenie nowej bazy danych (**MyDB1**) na **ContosoSQL1**

- Zarówno pełnej kopii zapasowej i kopii zapasowej dziennika transakcji bazy danych

- Przywracanie pełnej i logowania kopie zapasowe **ContosoSQL2** z opcją **NORECOVERY**

- Tworzenie grupy dostępności (**AG1**) z synchroniczne Zatwierdź, automatyczne przejście i czytelne repliki pomocniczej

### <a name="create-the-mydb1-database-on-contososql1"></a>Tworzenie bazy danych MyDB1 na ContosoSQL1:

1. Jeśli użytkownik nie już zalogował się z sesji pulpitu zdalnego **ContosoSQL1** i **ContosoSQL2**, zrób to teraz.

1. Uruchamianie pliku RDP **ContosoSQL1** i zaloguj się jako **CORP\Install**.

1. W **Eksploratorze plików**w obszarze * *C:\**, Utwórz katalog * *kopii zapasowej**. Użyj tego katalogu za pomocą kopii zapasowych i przywracania bazy danych.

1. Kliknij prawym przyciskiem myszy nowy katalog, wskaż polecenie **Udostępnij**, a następnie kliknij **konkretne osoby**, tak jak pokazano poniżej.

    ![Tworzenie kopii zapasowej folderu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)

1. Dodawanie **CORP\SQLSvc1** i nadaj uprawnienia do **Odczytu/zapisu** , a następnie dodaj **CORP\SQLSvc2** i nadaj uprawnienia do **odczytu** , tak jak pokazano poniżej, a następnie kliknij **Udostępnij**. Po zakończeniu procesu udostępniania plików, kliknij przycisk **Gotowe**.

    ![Udzielanie uprawnień do folderu kopii zapasowej](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)

1. Następnie możesz utworzyć bazę danych. W **Start** menu Uruchom **Program SQL Server Management Studio**, a następnie kliknij pozycję **Połącz** , aby nawiązać połączenie z domyślnym wystąpieniem programu SQL Server.

1. W **Eksploratorze obiektów**kliknij prawym przyciskiem myszy **baz danych** , a następnie kliknij przycisk **Nowej bazy danych**.

1. W polu **Nazwa bazy danych**wpisz **MyDB1**, a następnie kliknij przycisk **OK**.

### <a name="take-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Wykonaj kopię zapasową MyDB1 pełnego i przywrócenie jej na ContosoSQL2:

1. Następnie możesz wykonać pełne kopii zapasowej bazy danych. W **Eksploratorze obiektów**rozwiń węzeł **bazy danych**, a następnie kliknij prawym przyciskiem myszy **MyDB1**, a następnie wskaż polecenie **zadania**, a następnie kliknij **Wykonywanie kopii zapasowej**.

1. W sekcji **źródło** Zachowaj ustawieniu **Pełny** **Typ kopii zapasowej** . W sekcji **miejsce docelowe** kliknij przycisk **Usuń** , aby usunąć domyślna ścieżka pliku dla pliku kopii zapasowej.

1. W sekcji **miejsce docelowe** kliknij przycisk **Dodaj**.

1. W polu tekstowym **Nazwa pliku** wpisz ** \\ContosoSQL1\backup\MyDB1.bak**. Następnie kliknij **przycisk OK**, a następnie kliknij przycisk **OK** ponownie, aby utworzyć kopię zapasową bazy danych. Po zakończeniu operacji wykonywania kopii zapasowej, kliknij przycisk **OK** , aby zamknąć okno dialogowe.

1. Następnie możesz wykonać dziennik transakcji kopii zapasowej bazy danych. W **Eksploratorze obiektów**rozwiń węzeł **bazy danych**, a następnie kliknij prawym przyciskiem myszy **MyDB1**, a następnie wskaż polecenie **zadania**, a następnie kliknij **Wykonywanie kopii zapasowej**.

1. W polu Typ **kopii zapasowej** wybierz **Dziennik transakcji**. Zachowaj ścieżka pliku **docelowego** , ustaw jedną określonej wcześniej i kliknij **przycisk OK**. Po wykonaniu operacji wykonywania kopii zapasowej, kliknij przycisk **OK** .

1. Następnie możesz przywrócić kopie zapasowe dziennika pełny i transakcji na **ContosoSQL2**. Uruchamianie pliku RDP **ContosoSQL2** i zaloguj się jako **CORP\Install**. Pozostaw otwarte sesji pulpitu zdalnego dla **ContosoSQL1** .

1. W **Start** menu Uruchom **Program SQL Server Management Studio**, a następnie kliknij pozycję **Połącz** , aby nawiązać połączenie z domyślnym wystąpieniem programu SQL Server.

1. W **Eksploratorze obiektów**kliknij prawym przyciskiem myszy **baz danych** , a następnie kliknij pozycję **Przywróć bazę danych**.

1. W sekcji **źródło** wybierz **urządzenie**, a następnie kliknij przycisk **...** przycisk.

1. **Wybieranie urządzeń kopii zapasowej**kliknij przycisk **Dodaj**.

1. W polu Lokalizacja pliku kopii zapasowej, wpisz \\ContosoSQL1\backup, następnie kliknij pozycję Odśwież, a następnie wybierz MyDB1.bak, a następnie kliknij przycisk OK, a następnie kliknij przycisk OK. Powinien zostać wyświetlony pełnej kopii zapasowej i zestawów kopii zapasowej w kopii zapasowej do przywrócenia okienka.

1. Przejdź do strony Opcje, a następnie wybierz pozycję Przywróć z NORECOVERY w stanie odzyskiwania, a następnie kliknij przycisk OK, aby przywrócić bazę danych. Po wykonaniu operacji przywracania kliknij przycisk OK.

### <a name="create-the-availability-group"></a>Tworzenie grupy dostępności:

1. Wróć do sesji pulpitu zdalnego dla **ContosoSQL1**. W **Eksploratorze obiektów** w SSMS kliknij prawym przyciskiem myszy **Zawsze na wysokiej dostępności** i kliknij pozycję **Kreator nowej grupy dostępności**, tak jak pokazano poniżej.

    ![Uruchamianie Kreatora nowej grupy dostępności](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)

1. Na stronie **wprowadzenie** kliknij przycisk **Dalej**. Na stronie **Określ nazwę grupy dostępności** wpisz **AG1** w polu **Nazwa grupy dostępności**, a następnie kliknij przycisk **Dalej** .

    ![Kreator nowego AG, określ nazwę AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)

1. Na stronie **Wybieranie bazy danych** wybierz pozycję **MyDB1** , a następnie kliknij przycisk **Dalej**. Baza danych spełnia wymagania wstępne dla grupy dostępności, ponieważ miały co najmniej jeden pełnej kopii zapasowej na zamierzonego replice podstawowego.

    ![Kreator nowego AG, zaznacz bazy danych](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)

1. Na stronie **Określanie repliki** kliknij pozycję **Dodaj replice**.

    ![Kreator nowego AG, określ repliki](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)

1. Okno dialogowe **połączenie z serwerem** dzwoni. Wpisz **ContosoSQL2** w polu **Nazwa serwera**, a następnie kliknij przycisk **Połącz**.

    ![Kreator nowego AG, łączenie się z serwerem](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)

1. Ponownie na stronie **Określanie repliki** powinien zostać wyświetlony **ContosoSQL2** w **Replikach dostępnych**na liście. Konfigurowanie repliki, tak jak pokazano poniżej. Gdy skończysz, kliknij przycisk **Dalej**.

    ![Kreator nowego AG, określ repliki ukończono)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)

1. Na stronie **Wybierz pozycję początkowej synchronizacji danych** zaznacz **tylko dołączyć** , a następnie kliknij przycisk **Dalej**. Już wykonanych synchronizacji danych ręcznie po wykonanymi pełnej i transakcji kopii zapasowych **ContosoSQL1** i przywrócić je na **ContosoSQL2**. Zamiast tego możesz wykonywania kopii zapasowej i przywracanie operacje w bazie danych i wybierz pozycję **Pełna** umożliwia wykonywanie synchronizacji danych dla Ciebie Kreatora nowej grupy dostępności. Nie jest to jednak zalecane dla bardzo dużych baz danych, które znajdują się w niektórych firmach.

    ![Kreator nowego AG, zaznacz dane początkowe synchronizacji](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)

1. Na stronie **Sprawdzanie poprawności** kliknij przycisk **Dalej**. Ta strona powinna wyglądać podobnie do poniżej. Jest ostrzeżenie o Konfiguracja odbiornika, ponieważ nie skonfigurowano detektor grupy dostępności. Można zignorować to ostrzeżenie, ponieważ ten samouczek nieskonfigurowane detektor. Aby skonfigurować odbiornika ten samouczek, zobacz [Konfigurowanie detektor ILB zawsze na dostępność grup w Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

    ![Kreator nowego AG, sprawdzania poprawności](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)

1. Na stronie **podsumowania** kliknij przycisk **Zakończ**, a następnie zaczekaj, aż Kreator konfiguruje nowej grupy dostępności. Na stronie **informacje o postępie** kliknij pozycję **więcej szczegółów** , aby wyświetlić szczegółowe informacje o postępie. Po zakończeniu działania kreatora sprawdź, czy **wyników** wyszukiwania, aby sprawdzić, czy grupy dostępności pomyślnie jest utworzone, tak jak pokazano poniżej, a następnie kliknij przycisk **Zamknij** , aby zamknąć kreatora.

    ![Kreator nowego AG, wyników](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)

1. W **Eksploratorze obiektów**rozwiń pozycję **Zawsze na wysokiej dostępności**, a następnie rozwiń **Grupy dostępności**. Powinien zostać wyświetlony nowej grupy dostępność w tym kontenerze. Kliknij prawym przyciskiem myszy **AG1 (podstawowe)** i kliknij pozycję **Pokaż pulpit nawigacyjny**.

    ![Pokazywanie AG pulpitu nawigacyjnego](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)

1. **Zawsze na pulpicie nawigacyjnym** powinna wyglądać podobnie do tego, jak pokazano poniżej. Można zobaczyć repliki, tryb pracy awaryjnej każdej replice i stan synchronizacji.

    ![Pulpit nawigacyjny AG](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)

1. Powrót do **Menedżera serwera**, wybierz pozycję **Narzędzia**, a następnie uruchom **Menedżera klaster pracy awaryjnej**.

1. Rozwiń **Cluster1.corp.contoso.com**, a następnie rozwiń węzeł **usługi i aplikacje**. Wybierz **role** i pamiętaj, że zostały utworzone roli **AG1** Dostępność grupy. Należy zauważyć, że AG1 nie ma dowolny adres IP przez bazę danych, która klienci mogą nawiązywać połączenia do grupy dostępności, ponieważ nie skonfigurowane detektor. Umożliwia nawiązanie połączenia bezpośrednio do podstawowego węzeł operacje odczytu i zapisu i pomocniczą węzeł kwerendami tylko do odczytu.

    ![AG w Menedżerze klaster pracy awaryjnej](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

>[AZURE.WARNING] Nie próbuj awarię w grupie dostępności za pomocą Menedżera klaster pracy awaryjnej. Wszystkie operacje awaryjnego przejmowania zadań powinny zostać wykonane z **Zawsze na pulpicie nawigacyjnym** w SSMS. Aby uzyskać więcej informacji zobacz [ograniczenia na przy użyciu WSFC Menedżer klastrów pracy awaryjnej z grupami dostępności](https://msdn.microsoft.com/library/ff929171.aspx).

## <a name="next-steps"></a>Następne kroki
Możesz teraz pomyślnie wdrożono program SQL Server zawsze włączone, tworząc grupy dostępności platformy Azure. Aby skonfigurować detektor dla tej grupy dostępności, zobacz [Konfigurowanie detektor ILB zawsze na dostępność grup w Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Aby uzyskać inne informacje o korzystaniu z programu SQL Server w Azure zobacz [Programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
