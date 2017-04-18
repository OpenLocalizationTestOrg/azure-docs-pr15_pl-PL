<properties
    pageTitle="Konfigurowanie zawsze na grupy dostępności w maszyn wirtualnych Azure automatycznie – Menedżera zasobów"
    description="Tworzenie grupy zawsze na dostępność Azure maszyn wirtualnych w trybie Azure Menedżera zasobów. Interfejs użytkownika przede wszystkim samouczku automatycznego tworzenia całego rozwiązania."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Konfigurowanie zawsze na grupy dostępności w maszyn wirtualnych Azure automatycznie – Menedżera zasobów

> [AZURE.SELECTOR]
- [Menedżer zasobów: szablonu](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Menedżer zasobów: ręczne](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasyczny: interfejs użytkownika](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasyczny: programu PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Ten samouczek w końcu do końca pokazano, jak utworzyć grupę dostępność programu SQL Server przy użyciu Menedżera zasobów Azure maszyn wirtualnych. Samouczek użyto Azure karty, aby skonfigurować szablon. Będzie Przegląd ustawień domyślnych, wpisz wymagane ustawienia i zaktualizować karty w portalu, jak szczegółową tego samouczka.

Na koniec samouczka rozwiązanie programu SQL Server Dostępność grupy platformy Azure będzie się składał z następujących elementów:

- Wirtualna sieć zawierająca wiele podsieci, łącznie z zewnętrzną i podsieć wewnętrznej

- Kontroler dwie domeny z domeną usługi Active Directory (AD)

- Dwa SQL Server maszyny wirtualne wdrożyć podsieci wewnętrznej i dołączony do domeny AD

- Klaster WSFC węzeł 3 z modelem kworum Większość węzłów

- Grupy dostępności z dwoma replikami synchroniczne Zatwierdź dostępność bazy danych

Na poniższym rysunku jest graficzna reprezentacja rozwiązanie.

![Testowanie architektura ćwiczenia AG platformy Azure](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Wszystkie zasoby, w tym rozwiązaniu należeć do pojedynczej grupy zasobów.

Tego samouczka przyjęto założenie następujące czynności:

- Masz już konto Azure. Jeśli nie istnieje, [Utwórz konto wersji próbnej](http://azure.microsoft.com/pricing/free-trial/).

- Wiesz już, jak inicjować obsługę maszyn wirtualnych programu SQL Server, z galerii maszyn wirtualnych przy użyciu graficznego interfejsu użytkownika. Aby uzyskać więcej informacji zobacz [inicjowania obsługi administracyjnej maszyny wirtualnej programu SQL Server Azure](virtual-machines-windows-portal-sql-server-provision.md)

- Masz już pełny opis grup dostępności. Aby uzyskać więcej informacji zobacz [zawsze na grupy dostępności (program SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Jeśli interesuje Cię grupy dostępności za pomocą programu SharePoint, również zobacz [Konfigurowanie programu SQL Server 2012 zawsze na grupy dostępność dla programu SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).

W tym samouczku użyje portal Azure, aby:

- Wybierz szablon zawsze włączone z portalu

- Przejrzyj ustawienia szablonu i zaktualizuj kilka ustawień konfiguracji w środowisku usługi

- Monitorowanie Azure podczas tworzenia całego środowiska

- Nawiązywanie połączenia z jedną kontroler domeny, a następnie do jednego z serwerów SQL

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>Obsługa administracyjna klaster z galerii

Azure przewiduje rozwiązanie całego obrazu z galerii. Aby zlokalizować szablon:

1.  Zaloguj się do portalu Azure za pomocą konta.
1.  W portalu Azure kliknij polecenie **+ nowe.** Portalu zostanie otwarte nowe karta.
1.  Na nowe wyszukiwanie karta **(AlwaysOn)**.
![Znajdowanie szablonów (AlwaysOn)](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  W wynikach wyszukiwania Znajdź **Klaster programu SQL Server (AlwaysOn)**.
![Szablon (AlwaysOn)](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  Na **Wybierz model wdrożenia** wybierz **Menedżera zasobów**.

### <a name="basics"></a>Podstawowe informacje

Kliknij na **podstawy** i skonfigurować następujące ustawienia:

- **Nazwa użytkownika administratora** to konto użytkownika z uprawnieniami administratora domeny i należeć do roli serwera stały grupy programu SQL Server na oba wystąpienia programu SQL Server. Ten samouczek za pomocą **administrator domeny**.

- **Hasło** jest hasło do konta administratora domeny. Za pomocą hasła złożonego. Potwierdź hasło.

- **Subskrypcja** jest subskrypcję, do której Azure będzie BOM do wszystkich wdrożony w grupie dostępności zasobów. Jeśli Twoje konto ma wiele subskrypcji, możesz określić innej subskrypcji.

- **Grupa zasobów** jest nazwą dla grupy wszystkie zasoby Azure utworzony przez tego samouczka będzie znajdować się. Ten samouczek za pomocą **SQL-HA-RG**. Aby uzyskać więcej informacji, zobacz (Omówienie Menedżera zasobów Azure) [zasobów — Grupa overview.md-#-grup zasobów].

- **Lokalizacja** jest Azure region, w której zostanie utworzony zasoby dla tego samouczka. Wybierz Azure region do obsługi infrastruktury.

Poniżej znajduje się, jak będą wyglądały karta **— informacje podstawowe** :

![Podstawowe informacje](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Kliknij **przycisk OK**.

### <a name="domain-and-network-settings"></a>Ustawienia domeny i sieci

Ten szablon Azure galerii tworzy nową domenę z nowych kontrolerów domen. Tworzy również nową sieć i dwóch podsieci. Szablon nie umożliwia tworzenia serwerów w istniejącej domeny lub wirtualnej sieci. Następnym krokiem jest skonfigurowanie ustawień domeny i sieci.

Ustawienia **domeny i sieci** karta Przejrzyj wstępnie ustawionych wartości ustawień domeny i sieci:

- **Nazwa domeny głównej las** jest nazwę domeny, która będzie używana dla domeny AD, który będzie obsługiwać klaster. Samouczek za pomocą **contoso.com**.

- **Nazwa wirtualnej sieci** jest nazwą sieciową Azure wirtualnej sieci. Ten samouczek za pomocą **autohaVNET**.

- **Nazwa podsieci kontrolera domeny** jest nazwą części wirtualnej sieci, który obsługuje kontrolera domeny. Ten samouczek za pomocą **podsieci 1**. Tej podsieci będzie używany prefiks adresu **10.0.0.0/24**.

- **Nazwa podsieci programu SQL Server** jest nazwą części wirtualnej sieci, że hostów serwerów SQL i plik udostępnianie monitora. Ten samouczek za pomocą **podsieci 2**. Tej podsieci będzie używany prefiks adresu **10.0.1.0/26**.

Aby dowiedzieć się więcej na temat wirtualnych sieci w [Azure, zobacz Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md).  

**Ustawienia domeny i sieci** powinna wyglądać następująco:

![Ustawienia domeny i sieci](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

W razie potrzeby możesz zmienić tych wartości. Ten samouczek firma Microsoft korzysta z istniejących wartości.

- Przejrzyj ustawienia, a następnie kliknij **przycisk OK**.

###<a name="availability-group-settings"></a>Ustawienia grupy dostępności

Na **Ustawienia grupy dostępność** Przejrzyj wstępnie ustawionych wartości dla grupy dostępności i odbiornika.

- **Nazwa grupy dostępności** jest nazwa zasobu grupowany dla grupy dostępności. Ten samouczek za pomocą **Contoso-ag**.

- **Dostępność grupy odbiornika nazwa** jest używana przez klaster i równoważenia obciążenia wewnętrzny. Klientów łączących się z programu SQL Server umożliwia łączenie odpowiednie replice bazy danych tej nazwy. Ten samouczek za pomocą **odbiornika Contoso**.

-  **Dostępność grupy odbiornika port** Określa, że użyje port TCP odbiornika programu SQL Server. Ten samouczek za pomocą domyślny port **1433**.

W razie potrzeby możesz zmienić tych wartości. Ten samouczek za pomocą wstępnie ustawionych wartości.  

![Ustawienia grupy dostępności](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Kliknij **przycisk OK**.

###<a name="vm-size-storage-settings"></a>Rozmiar pamięci Wirtualnej, miejsca do magazynowania

**Rozmiar pamięci Wirtualnej** , miejsca do magazynowania wybierz rozmiar maszyn wirtualnych programu SQL Server i przejrzyj inne ustawienia.

- **Rozmiar maszyn wirtualnych programu SQL Server** jest rozmiarem Azure maszyn wirtualnych zarówno serwera SQL. Wybierz odpowiednie dla Twojej obciążenie pracą rozmiar maszyn wirtualnych. Jeśli tworzysz za pomocą tego środowiska samouczka **DS2**. Produkcji obciążenia wybierz rozmiar maszyn wirtualnych obsługujące obciążenie pracą. Wiele obciążenia produkcji wymaga **DS4** lub większej. Szablon zostanie tworzenie dwóch maszyn wirtualnych o tym rozmiarze i zainstalować programu SQL Server na każdej z nich. Aby uzyskać więcej informacji zobacz [rozmiarów maszyn wirtualnych](virtual-machines-linux-sizes.md).

>[AZURE.NOTE]Azure zainstaluje Enterprise Edition programu SQL Server. Koszt zależy od wersji i rozmiar maszyn wirtualnych. Aby uzyskać szczegółowe informacje dotyczące kosztów bieżącej zobacz [ceny maszyn wirtualnych](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

- **Rozmiar maszyn wirtualnych kontrolera domeny** jest wielkością maszyn wirtualnych kontrolerów domeny. Ten samouczek za pomocą **D2**.

- **Rozmiar maszyn wirtualnych monitora udostępniania plików** jest rozmiarem maszyn wirtualnych dla monitora udostępniania plików. Ten samouczek za pomocą **A1**.

- **Konto magazynu SQL** jest nazwę konta magazynu do przechowywania danych programu SQL Server i systemu operacyjnego dysków. Ten samouczek za pomocą **alwaysonsql01**.

- Nazwa konta miejsca do magazynowania dla kontrolerów domeny jest **konto magazynowania kontrolera domeny** . Ten samouczek za pomocą **alwaysondc01**.

- **Dane programu SQL Server dysku rozmiar** TB jest wielkością dysku danych programu SQL Server w TB. Umożliwia określenie liczby od 1 do 4. To jest rozmiar dysku danych, które zostaną dołączone do każdego programu SQL Server. Skorzystaj z tego samouczka **1**.

- **Optymalizacja przestrzeni dyskowej** Ustawia ustawienia konfiguracji określonego miejsca do magazynowania dla maszyn wirtualnych programu SQL Server zależy od typu obciążenie pracą. Wszystkie serwery SQL w tym scenariuszu za pomocą premium przestrzeni dyskowej pamięci podręcznej hosta Azure dysku ustawić tylko do odczytu. Ponadto może optymalizować ustawienia programu SQL Server dla obciążenie pracą, wybierając jeden z tych trzech ustawień:

    - **Ogólne obciążenie pracą** zestawów nie określone ustawienia konfiguracji

    - **Przetwarzanie transakcyjna** ustawia flagę śledzenia 1117 i 1118

    - **Magazynowanie danych** ustawia flagę śledzenia 1117 i 610

Ten samouczek za pomocą **Ogólne obciążenie pracą**.

![Rozmiar maszyn wirtualnych miejsca do magazynowania](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Przejrzyj ustawienia, a następnie kliknij **przycisk OK**.

####<a name="a-note-about-storage"></a>Uwaga dotycząca miejsca do magazynowania

Dodatkowe optymalizacje zależy od rozmiaru dysków danych programu SQL Server. Dla każdego terabajtów dysku danych Azure dodaje dodatkowe miejsce premium 1 TB (SSD). Jeśli serwer wymaga 2 TB lub więcej, szablon utworzy puli miejsca do magazynowania poszczególnych programu SQL Server. Puli miejsca do magazynowania jest formularz wirtualizacji miejsca do magazynowania, gdzie wiele dysków są skonfigurowane do dostarczania wyższa wydajność, elastyczność i wydajności.  Następnie szablon utworzy ilości miejsca do magazynowania puli miejsca do magazynowania i przedstawia to jako pojedynczy danych do systemu operacyjnego. Szablon określa dysku jako dysku danych dla programu SQL Server. Szablon optymalizacja działania puli miejsca do magazynowania dla programu SQL Server przy użyciu następujących ustawień:

- Rozmiar pasek to ustawienie przeplotu wirtualnego dysku. Dla transakcji obciążenia to jest ustawiony na 64 KB. Dane składu obciążenia ustawienie jest 256 KB.

- Elastyczność jest proste (nie elastyczność).

>[AZURE.NOTE] Magazyn Azure premium jest zbędne lokalnie i zachowuje trzy kopie danych w jednym regionie, więc dodatkowej elastyczności w puli miejsca do magazynowania nie jest wymagane.

- Liczba kolumn jest równa liczbie dysków w puli miejsca do magazynowania.

Aby uzyskać dodatkowe informacje dotyczące magazynu miejsca i miejsca do magazynowania pule Zobacz:

- [Omówienie spacje magazynu](http://technet.microsoft.com/library/hh831739.aspx).

- [Kopia zapasowa systemu Windows Server i pule miejsca do magazynowania](http://technet.microsoft.com/library/dn390929.aspx)

Aby uzyskać więcej informacji o najważniejszych wskazówkach konfiguracji programu SQL Server zobacz [Wydajność najważniejsze wskazówki dotyczące programu SQL Server w środowisku maszyn wirtualnych Azure](virtual-machines-windows-sql-performance.md)


###<a name="sql-server-settings"></a>Ustawienia programu SQL Server

Ustawienia **Programu SQL Server** Przejrzyj i zmodyfikuj prefiks nazwy maszyn wirtualnych programu SQL Server, wersja programu SQL Server, konta usługi programu SQL Server i hasło i automatycznie SQL poprawki harmonogramie konserwacji.

- **Prefiks programu SQL Server nazwy** umożliwia tworzenie nazwy dla każdego programu SQL Server. Ten samouczek za pomocą **Contoso-ag**. Nazwy programu SQL Server będzie *Contoso-ag-0* i *Contoso-ag-1*.

- **Wersja programu SQL Server** jest wersja programu SQL Server. Ten samouczek za pomocą **programu SQL Server w 2014 r**. Możesz również wybrać **programu SQL Server 2012** lub **SQL Server 2016**.

- **Nazwa użytkownika konta usługi programu SQL Server** jest nazwą konta domeny dla usługi SQL Server. Ten samouczek za pomocą **sqlservice**.

- **Hasło** jest hasło do konta usługi programu SQL Server.  Za pomocą hasła złożonego. Potwierdź hasło.

- **Poprawki automatycznej SQL harmonogramu konserwacji** identyfikuje dzień tygodnia, że Azure będzie automatycznie poprawka serwerów SQL. W tym samouczku wpisz **niedziela**.

- **Godzina rozpoczęcia konserwacji poprawki automatycznej SQL** jest godzina Azure regionu, gdy rozpocznie się automatyczne poprawianie.

>[AZURE.NOTE]Okno poprawką dla każdego maszyn wirtualnych jest rozłożone przez godzinę. Tylko jedna maszyna wirtualna jest poprawkami w danej chwili w celu uniknięcia zakłóceń usług.

![Ustawienia programu SQL Server](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Przejrzyj ustawienia, a następnie kliknij **przycisk OK**.

###<a name="summary"></a>Podsumowanie

Na stronie Podsumowanie Azure sprawdza ustawienia. Możesz również pobrać szablon. Zapoznaj się z podsumowaniem. Kliknij **przycisk OK**.

###<a name="buy"></a>Kupowanie

Ta wersja ostateczna karta zawiera **warunki użytkowania**i **Zasady zachowania poufności informacji**. Przejrzyj te informacje. Gdy Azure, aby rozpocząć tworzenie maszyn wirtualnych i wszystkie inne wymagane zasoby w grupie dostępności, kliknij przycisk **Utwórz**.

Azure portal utworzy grupy zasobów i wszystkie zasoby.

##<a name="monitor-deployment"></a>Monitorowanie wdrażania

Monitorowanie postępu wdrażania z portalu Azure. Ikona oznaczająca rozmieszczania zostanie automatycznie przypięta do Azure portalu pulpitu nawigacyjnego.

![Azure pulpitu nawigacyjnego](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>Nawiązywanie połączenia z programem SQL Server

Nowe wystąpienia programu SQL Server są uruchomione na maszyn wirtualnych, które nie masz połączenia z Internetem. Kontrolery domeny jednak przeciwległych połączenia internetowego. Aby połączyć się z serwerami SQL z pulpitu zdalnego, pierwszy RDP do jednego kontroler domeny. Z kontrolera domeny otwórz drugi RDP do programu SQL Server.

Aby RDP do podstawowego kontrolera domeny wykonaj następujące czynności:

1.  Z Azure portalu pulpitu nawigacyjnego bardzo powiodła się wdrożenia.

1.  Kliknij pozycję **zasoby**.

1.  Karta **zasoby** kliknij **ad podstawowa-kontrolera domeny** jest nazwą komputera maszyny wirtualnej podstawowego kontrolera domeny.

1.  Na karta dla **ad podstawowa-kontrolera domeny** kliknij przycisk **Połącz**. Przeglądarki zostanie wyświetlone pytanie, czy chcesz otworzyć lub zapisać obiekt połączenia zdalnego. Kliknij przycisk **Otwórz**.
![Nawiązywanie połączenia z kontrolera domeny](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Podłączanie pulpitu zdalnego** , może być ostrzeżenie, że nie można zidentyfikować wydawcy tego połączenia zdalnego. Kliknij przycisk **Połącz**.

1.  Zabezpieczenia systemu Windows jest wyświetlany monit o podanie poświadczeń, aby nawiązać połączenie adres IP podstawowego kontrolera domeny. Kliknij opcję **Użyj innego konta**. **Nazwa użytkownika** wpisz **contoso\DomainAdmin**. Jest to konto, wybrana dla nazwy użytkownika administratora. Za pomocą hasła złożonego wybranej podczas konfigurowania szablonu.

1.  **Pulpit zdalny** może ostrzeżenie, że nie można uwierzytelnić komputera zdalnego z powodu problemów z jego certyfikatem zabezpieczeń. Zostanie wyświetlona nazwa certyfikatu zabezpieczeń. Po wykonaniu samouczka nazwa będzie **ad podstawowa dc.contoso.com**. Kliknij przycisk **Tak**.

Możesz teraz połączenie podstawowego kontrolera domeny. Aby RDP do programu SQL Server wykonaj następujące czynności:

1.  W witrynie kontrolera domeny otwórz **Podłączanie pulpitu zdalnego**.

1.  Na **komputerze**wpisz nazwę jednego z serwerów SQL. Ten samouczek wpisz wartość **0 SQL**.

1.  Za pomocą tego samego konta użytkownika i hasło, którego użyto do RDP do kontrolera domeny.

Teraz masz połączenie z RDP do programu SQL Server. Możesz Otwórz program SQL Server management studio, łączenie się z domyślnym wystąpieniem programu SQL Server i upewnij się, że skonfigurowano grupie availabilty.


