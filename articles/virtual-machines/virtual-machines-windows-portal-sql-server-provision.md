<properties
    pageTitle="Inicjowanie obsługi maszyny wirtualnej SQL Server | Microsoft Azure"
    description="Tworzenie i połączyć się z komputerem wirtualnych programu SQL Server w Azure za pomocą portalu. Ten samouczek tryb Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Inicjowanie obsługi programu SQL Server maszyny wirtualnej Azure Portal

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [Programu PowerShell](virtual-machines-windows-ps-sql-create.md)

Ten samouczek w końcu do końca pokazano, jak za pomocą Azure Portal obsługi administracyjnej maszyny wirtualnej uruchomiony program SQL Server.

Galeria Azure maszyn wirtualnych (maszyn wirtualnych) zawiera kilka obrazów, które zawierają programu Microsoft SQL Server. Za pomocą kilku kliknięć można wybrać jedną z obrazów maszyn wirtualnych SQL z galerii i umożliwić jego w środowisku usługi Azure.

W tym samouczku dowiesz się:

- [Wybierz obraz maszyn wirtualnych SQL z galerii](#select-a-sql-vm-image-from-the-gallery)
- [Konfigurowanie i Tworzenie maszyn wirtualnych](#configure-the-vm)
- [Otwieranie maszyn wirtualnych z pulpitu zdalnego](#open-the-vm-with-remote-desktop)
- [Nawiązywanie połączenia z programem SQL Server](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Wybierz obraz maszyn wirtualnych SQL z galerii

1. Zaloguj się do [portalu Azure](https://portal.azure.com) za pomocą konta.

    >[AZURE.NOTE] Jeśli nie masz konta usługi Azure, odwiedź [Azure bezpłatny okres próbny](https://azure.microsoft.com/pricing/free-trial/).

1. W portalu Azure kliknij przycisk **Nowy**. Portalu zostanie otwarta **Nowa** karta. Zasoby maszyn wirtualnych programu SQL Server są w grupie **maszyn wirtualnych** Marketplace.

1. Karta **Nowy** kliknij **maszyn wirtualnych**.

1. Aby wyświetlić wszystkie dostępne obrazy, wybierz polecenie **wyświetlić wszystkie** karta **maszyn wirtualnych** .

    ![Karta Azure maszyn wirtualnych](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. W obszarze **serwer bazy danych**kliknij pozycję **SQL Server**. Może być konieczne przewinięcie w dół, aby zlokalizować **serwer bazy danych**. Przejrzyj dostępne szablony programu SQL Server.

    ![Galeria komputera wirtualnego SQL obrazów](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Każdy szablon służy do identyfikowania wersja programu SQL Server i systemu operacyjnego. Wybierz jeden z tych obrazów z listy. Następnie przejrzyj kartę Szczegóły, która zawiera opis obrazu maszyn wirtualnych.

    >[AZURE.NOTE] Obrazów maszyn wirtualnych SQL obejmują koszty licencjonowania dla programu SQL Server do ceny na minutę maszyn wirtualnych, możesz utworzyć. Istnieje inną opcję, aby wyświetlić swój właścicielem licencji (BYOL) i opłacanie tylko w przypadku maszyn wirtualnych. Obraz nazw są prefiksem {BYOL}. Aby uzyskać więcej informacji na temat tej opcji zobacz [Wprowadzenie do programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).

1. W obszarze **Wybierz model wdrożenia**Sprawdź, czy zaznaczono **Menedżera zasobów** . Menedżer zasobów jest model wdrożenia zalecane dla nowych maszyn wirtualnych. Kliknij przycisk **Utwórz**.

    ![Tworzenie maszyn wirtualnych SQL przy użyciu Menedżera zasobów](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>Konfigurowanie maszyn wirtualnych
Istnieje pięć karty konfigurowania maszyny wirtualnej programu SQL Server.

| Krok               | Opis                          |
|---------------------|-------------------------------|
| **Podstawowe informacje**              | [Konfigurowanie ustawień podstawowe](#1-configure-basic-settings)      |
| **Rozmiar**                | [Wybieranie rozmiaru maszyn wirtualnych](#2-choose-virtual-machine-size)   |
| **Ustawienia**            | [Konfigurowanie funkcji opcjonalne](#3-configure-optional-features)   |
| **Ustawienia programu SQL Server** | [Konfigurowanie ustawień serwera SQL](#4-configure-sql-server-settings) |
| **Podsumowanie**             | [Zapoznaj się z podsumowaniem](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. Konfigurowanie podstawowych ustawień
Na karta **— informacje podstawowe** wprowadź następujące informacje:

* Wpisz unikatową **nazwę**maszyny wirtualnej.
* Określ **nazwę użytkownika** dla lokalnego konta administratora na maszyn wirtualnych. To konto również jest dodawany do roli serwera stały **grupy** programu SQL Server.
* Podaj silne **hasła**.
* Jeśli masz wiele subskrypcji, sprawdź, czy subskrypcja jest poprawny dla nowych maszyn wirtualnych.
* W polu **Grupa zasobów** wpisz nazwę nowej grupy zasobów. Można też kliknąć aby użyć istniejący zasób grupy kliknij pozycję **Wybierz istniejące**. Grupa zasobów to zbiór zasoby pokrewne w Azure (maszyn wirtualnych kont miejsca do magazynowania, wirtualnych sieci, itp.).

    >[AZURE.NOTE] Korzystanie z nowej grupy zasobów jest przydatne, jeśli tylko testów lub informacje na temat wdrażania programu SQL Server Azure. Po zakończeniu z testem, Usuń grupa zasobów, aby automatycznie usunąć maszyn wirtualnych i wszystkie zasoby skojarzone z danej grupy zasobów. Aby uzyskać więcej informacji dotyczących grup zasobów zobacz [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md).

* Wybierz **lokalizację** do tego wdrożenia.
* Kliknij **przycisk OK** , aby zapisać ustawienia.

    ![Karta podstawowe informacje o SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Wybierz rozmiar maszyn wirtualnych
W kroku **rozmiar** wybierz rozmiar maszyn wirtualnych w karta **Wybierz odpowiedni rozmiar** . Karta wyświetla początkowo rozmiar zalecany komputera na podstawie szablonu, który wybrano. Szacuje go też miesięcznych kosztów do uruchomienia maszyn wirtualnych.

![Opcje rozmiaru maszyn wirtualnych SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Obciążeń pracą produkcji zalecamy wybranie rozmiarowi maszyn wirtualnych, który pozwala na [Przechowywanie Premium](../storage/storage-premium-storage.md). Jeśli nie jest wymagane ten poziom wydajności, użyj przycisku **Wyświetl wszystkie** zawiera wszystkie opcje rozmiaru komputera. Na przykład można użyć mniejszy rozmiar maszynowego do rozwoju lub środowisku testowym.

>[AZURE.NOTE] Aby uzyskać więcej informacji na temat maszyn wirtualnych rozmiarów zobacz [rozmiarów maszyn wirtualnych](virtual-machines-windows-sizes.md). Aby uzyskać informacje na temat rozmiarów maszyn wirtualnych programu SQL Server zobacz [Wydajność najważniejsze wskazówki dotyczące programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-performance.md).

Wybierz rozmiar komputera, a następnie kliknij przycisk **Wybierz**.

## <a name="3-configure-optional-features"></a>3. Konfigurowanie opcjonalne funkcje
Na karta **Ustawienia** skonfigurować Azure magazyn, sieci i monitorowanie maszyny wirtualnej.

- W obszarze **miejsca do magazynowania**określ **dysku wpisz** standardowy lub Premium (SSD). Magazyn Premium jest zalecane dla obciążenia produkcji.

>[AZURE.NOTE] Jeśli wybierzesz Premium (SSD) o rozmiarze komputera, która nie obsługuje magazynowania Premium, automatycznie zmienia rozmiar komputera.  

- W obszarze **konta miejsca do magazynowania**możesz zaakceptować nazwę konta magazynu automatycznie ustanawianie. Możesz również kliknąć dla **konta miejsca do magazynowania** na wybieranie istniejącego konta i konfigurowanie przechowywania typu konta. Domyślnie Azure tworzy nowe konto miejsca do magazynowania z lokalnie zbędnych miejsca do magazynowania. Aby uzyskać więcej informacji na temat opcji przechowywania zobacz [replikacji magazyn Azure](../storage/storage-redundancy.md).

- W obszarze **sieć**możesz zaakceptować automatycznie wypełnione wartościami. Możesz również kliknąć na poszczególnych funkcji, aby ręcznie skonfigurować **wirtualnej sieci**, **podsieci**, **publiczny adres IP**i **Grupy zabezpieczeń sieci**. Na potrzeby tego samouczka zachować wartości domyślne.

- Azure umożliwia **Monitorowanie** domyślnie do tego samego konta miejsca do magazynowania, wyznaczona dla maszyn wirtualnych. Możesz zmienić te ustawienia w tym miejscu.

- W obszarze **Ustawianie dostępności**określ zestaw dostępności. Na potrzeby tego samouczka możesz wybrać **Brak**. Aby skonfigurować grupy dostępności (AlwaysOn) SQL, należy skonfigurować dostępność, aby uniknąć ponownego tworzenia maszyny wirtualnej.  Aby uzyskać więcej informacji zobacz [Zarządzanie dostępność maszyn wirtualnych](virtual-machines-windows-manage-availability.md).

Po zakończeniu konfigurowania tych ustawień, kliknij **przycisk OK**.

## <a name="4-configure-sql-server-settings"></a>4. Konfigurowanie ustawień serwera SQL
Na karta **Ustawienia programu SQL Server** Konfigurowanie określonych ustawień i optymalizacji w przypadku programu SQL Server. Ustawienia, które można skonfigurować dla programu SQL Server są następujące.

| Ustawienie               |
|---------------------|
| [Łączność](#connectivity)              |
| [Uwierzytelnianie](#authentication)                |
| [Konfiguracja magazynu](#storage-configuration)            |
| [Automatyczne poprawianie](#automated-patching) |
| [Automatyczne wykonywanie kopii zapasowych](#automated-backup)             |
| [Integracja Azure klucza magazynu](#azure-key-vault-integration)             |
| [Usługi R](#r-services) |

### <a name="connectivity"></a>Łączność
W obszarze **łączność SQL**Określ typ dostępu, który chcesz wystąpienie programu SQL Server, w tym maszyn wirtualnych. Na potrzeby tego samouczka zaznacz pozycję **publiczne (internet)** do zezwalania na połączenia z programem SQL Server z komputerów lub usług w Internecie. Ta opcja jest zaznaczona Azure automatycznie konfiguruje zapory lub grupę zabezpieczeń sieci, aby umożliwić ruch na porcie 1433.  

![Opcje połączeń SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Aby połączyć się z programem SQL Server za pośrednictwem Internetu, również musisz włączyć uwierzytelnianie programu SQL Server, które zostało opisane w następnej sekcji.

>[AZURE.NOTE] Użytkownik może dodać więcej ograniczeń komunikacji sieci do maszyn wirtualnych usługi SQL Server. Możesz to zrobić przez edytowanie grupy zabezpieczeń sieci po utworzeniu maszyn wirtualnych. Aby uzyskać więcej informacji, zobacz [Co to jest grupa zabezpieczeń sieci (NSG)?](../virtual-network/virtual-networks-nsg.md)

Jeśli wolisz nie włączenie połączeń aparatu bazy danych za pośrednictwem Internetu, wybierz jedną z następujących opcji:

- **Lokalne (wewnątrz maszyn wirtualnych tylko)** do zezwalania na połączenia z programem SQL Server tylko z poziomu maszyn wirtualnych.
- **Prywatne (w obszarze wirtualna sieć)** do zezwalania na połączenia z programem SQL Server z komputerów lub usług w tej samej sieci wirtualnej.

>[AZURE.NOTE] Obraz maszyn wirtualnych dla programu SQL Server Express edition nie powoduje automatycznego włączenia protokół TCP/IP. Dotyczy to nawet w przypadku opcji łączności publicznych i prywatnych. Dla wersji Express należy użyć Menedżer konfiguracji programu SQL Server do [ręcznie włączyć protokół TCP/IP](#configure-sql-server-to-listen-on-the-tcp-protocol) , po utworzeniu maszyn wirtualnych.

Ogólnie poprawić zabezpieczenia, wybierając pozycję największymi łączności, który umożliwia rozwiązania. Ale wszystkie opcje są zabezpieczanych przez zasady grupy zabezpieczeń sieci i uwierzytelniania SQL i systemu Windows.

Domyślnie **port** 1433. Można określić inny numer portu.
Aby uzyskać więcej informacji, zobacz [Łączenie do programu SQL Server maszyny wirtualnej (Menedżer zasobów) | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Uwierzytelnianie
Jeśli jest wymagane uwierzytelnianie programu SQL Server, kliknij opcję **Włącz** w obszarze **uwierzytelnianie SQL**.

![Uwierzytelnianie programu SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Jeśli planujesz uzyskać dostęp do programu SQL Server w Internecie (to znaczy opcja łączności publicznej w zakresie), należy włączyć uwierzytelnianie programu SQL w tym miejscu. Publiczny dostęp do programu SQL Server wymaga korzystania z uwierzytelniania SQL.

Jeśli włączysz uwierzytelnianie programu SQL Server, określ **nazwę logowania** i **hasło**. Ta nazwa użytkownika jest skonfigurowana jako logowania uwierzytelnianie programu SQL Server i członkiem stałej roli serwera **administratora systemu** . Aby uzyskać więcej informacji na temat tryby uwierzytelniania w temacie [Wybór tryb uwierzytelniania](http://msdn.microsoft.com/library/ms144284.aspx) .

Jeśli uwierzytelnianie programu SQL Server nie jest włączona, następnie umożliwia lokalnego konta administratora na maszyn wirtualnych połączyć się z wystąpieniem programu SQL Server.

### <a name="storage-configuration"></a>Konfiguracja magazynu
Kliknij pozycję **Konfiguracja magazynu** , aby określić wymagania dotyczące miejsca do magazynowania.

![Konfiguracja magazynu SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Jeśli wybierzesz standardowego magazynu, ta opcja nie jest dostępna. Optymalizacja automatycznego przechowywania jest dostępna tylko dla Premium miejsca do magazynowania.

Wymagania dotyczące można określić jako operacje wejścia i wyjścia na sekundę operacji i/o na (sekundę), przepustowość w Megabajtach na sekundę i rozmiar całkowitą ilość przestrzeni dyskowej. Skonfiguruj te wartości przy użyciu skali ruchomej. Portalu automatycznie oblicza liczbę dysków na podstawie tych wymagań.

Domyślnie Azure optymalizuje miejsca do magazynowania dla operacji i/o na sekundę 5000, 200 MB i 1 TB miejsca. Możesz zmienić te ustawienia miejsca do magazynowania na podstawie obciążenia. W obszarze **magazynowania zoptymalizowana pod kątem**wybierz jedną z następujących opcji:

- **Ogólne** jest to domyślne ustawienie i obsługuje większość obciążenia.
- Przetwarzanie **transakcyjna** optymalizuje magazyn obciążenia OLTP tradycyjnych bazy danych.
- **Magazynowanie danych** optymalizuje magazyn obciążenia analityczne i raportowania.

>[AZURE.NOTE] Limity górnym suwaki są zależne od rozmiar wybranego maszyn wirtualnych.

### <a name="automated-patching"></a>Automatyczne poprawianie
**Automatyczne poprawianie** jest domyślnie włączona. Automatyczne poprawianie umożliwia Azure, aby automatycznie poprawka programu SQL Server i systemu operacyjnego. Określ dzień tygodnia, godziny i czasu trwania dla okna konserwacji. Azure wykonuje poprawki w tym oknie konserwacji. Wykaz okien konserwacji korzysta z ustawień regionalnych maszyn wirtualnych raz. Jeśli nie chcesz, aby Azure, aby automatycznie poprawka programu SQL Server i systemu operacyjnego, kliknij przycisk **Wyłącz**.  

![SQL automatycznego poprawiania](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Aby uzyskać więcej informacji zobacz [Automatyczne poprawianie dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatyczne wykonywanie kopii zapasowych
Włączanie automatycznych kopii zapasowych dla wszystkich baz danych w obszarze **automatyczne wykonywanie kopii zapasowych**. Automatycznej kopii zapasowej jest domyślnie wyłączona.

Po włączeniu automatycznej kopii zapasowej SQL, możesz skonfigurować następujące czynności:

- Okres przechowywania (dni) kopii zapasowych
- Konta miejsca do magazynowania dla kopii zapasowych
- Opcja szyfrowania i hasło kopii zapasowych

Aby zaszyfrować kopii zapasowej, kliknij polecenie **Włącz**. Następnie określ **hasło**. Azure tworzy certyfikat szyfrowania kopii zapasowych i używa określonego hasła do ochrony tego certyfikatu.

![SQL zautomatyzowanego tworzenia kopii zapasowych](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Aby uzyskać więcej informacji zobacz [Automatyczne wykonywanie kopii zapasowych dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Integracja magazynu klucza Azure
Aby przechowywać hasła zabezpieczeń w Azure szyfrowania, kliknij **integracji Azure magazynu klucza** , a następnie kliknij polecenie **Włącz**.

![Integracja z SQL Azure klucza magazynu](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

W poniższej tabeli wymieniono parametry wymagane do skonfigurowania Azure klucza magazynu integracji.

|PARAMETR|OPIS|PRZYKŁAD|
|----------|----------|-------|
|**Adres URL klucza magazynu** |Lokalizacja klucza magazynu.|https://contosokeyvault.Vault.Azure.NET/ |
|**Główna nazwa** |Azure Active Directory głównej nazwy usługi. Ta nazwa jest określane jako identyfikator klienta.  |fde2b411 - 33d 5-4e11-af04eb07b669ccf2|
| **Tajny kapitału**|Azure Active Directory usługi kapitału hasło. To hasło jest także nazywane tajny klienta. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm-azF1XDKM =|
|**Nazwa poświadczeń**|**Nazwa poświadczeń**: integracja z programem AKV tworzy poświadczeń w ramach programu SQL Server, co pozwala maszyn wirtualnych mieć dostęp do klucza magazynu. Wybierz inną nazwę dla tego poświadczenia.| mycred1|

Aby uzyskać więcej informacji zobacz [Konfigurowanie Azure klucza magazynu integracja dla programu SQL Server na maszyny wirtualne Azure](virtual-machines-windows-ps-sql-keyvault.md).

Po zakończeniu konfigurowania ustawień programu SQL Server, kliknij **przycisk OK**.

### <a name="r-services"></a>Usługi R
Dla wersji SQL Server 2016 Enterprise masz możliwość włączenia [Usług SQL Server R](https://msdn.microsoft.com/library/mt604845.aspx). Pozwala na korzystanie z programu SQL Server 2016 zaawansowanej analizy. Kliknij opcję **Włącz** na karta **Ustawienia serwera SQL** .

![Włączanie usług SQL Server R](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] W przypadku obrazów programu SQL Server, które nie są 2016 Enterprise edition jest wyłączona, opcja Włącz usługi R.

## <a name="5-review-the-summary"></a>5. Przejrzyj podsumowanie
Karta **Podsumowanie** Przejrzyj podsumowanie i kliknij **przycisk OK** , aby utworzyć programu SQL Server, grupa zasobów i zasoby określone dla tego maszyn wirtualnych.

Można monitorować wdrażania z portalu azure. Przycisk **powiadomienia** u góry ekranu przedstawiono podstawowe stan wdrożenia.

>[AZURE.NOTE] Aby zapewnić możesz uzyskać ogólny obraz na czas wdrożenia, wdrażania maszyny SQL dla regionu wschodniego USA z ustawieniami domyślnymi. Ten test wdrożenia zajęła sumę 26 minut. Ale mogą wystąpić szybszą i tempo czas wdrażania na podstawie regionu wybrane ustawienia.

## <a name="open-the-vm-with-remote-desktop"></a>Otwieranie maszyn wirtualnych z pulpitu zdalnego

Aby połączyć się z komputerem wirtualnych z pulpitu zdalnego, wykonaj następujące czynności:

1. Po utworzeniu maszyn wirtualnych Azure na pulpitu nawigacyjnego Azure pojawi się ikona maszyn wirtualnych. Można również znaleźć, przeglądając istniejących maszyn wirtualnych. Kliknij na komputerze wirtualnych SQL. Karta **maszyn wirtualnych** Wyświetla szczegóły swojego maszyn wirtualnych.
1. W górnej części karta **maszyn wirtualnych** kliknij przycisk **Połącz**.
1. Przeglądarka plików do pobrania pliku RDP dla maszyn wirtualnych. Otwórz plik RDP.
    ![Pulpit zdalny do maszyn wirtualnych SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Podłączanie pulpitu zdalnego o tym, że nie można zidentyfikować wydawcy tego połączenia zdalnego. Kliknij przycisk **Połącz** , aby kontynuować.
1. W oknie dialogowym **Zabezpieczenia systemu Windows** kliknij opcję **Użyj innego konta**.
1. **Nazwa użytkownika** typu ** \<nazwa użytkownika >**, gdzie <user name> jest nazwą użytkownika określoną podczas konfigurowania maszyn wirtualnych. Należy dodać początkowej odwrotny przed nazwą.
1. Wpisz **hasło** , które wcześniej skonfigurowane dla tej maszyn wirtualnych, a następnie kliknij **przycisk OK** , aby połączyć.
1. Jeśli inne okno dialogowe **Podłączanie pulpitu zdalnego** poprosi Cię czy nawiązać połączenie, kliknij przycisk **Tak**.

Po nawiązaniu połączenia maszyn wirtualnych programu SQL Server można uruchomić program SQL Server Management Studio i połączyć się z uwierzytelniania systemu Windows przy użyciu poświadczeń administratora lokalnego. Jeśli zostanie włączona uwierzytelnianie programu SQL Server, w przypadku nawiązywania połączenia z uwierzytelnianiem SQL przy użyciu SQL logowania i hasło, które zostało skonfigurowane podczas inicjowania obsługi administracyjnej.

Dostęp do komputera można bezpośrednio zmienić komputera i ustawienia programu SQL Server, zgodnie z własnymi potrzebami. Na przykład możesz skonfigurować ustawienia zapory lub zmienić ustawienia konfiguracji programu SQL Server.

## <a name="connect-to-sql-server-remotely"></a>Nawiązywanie połączenia z programem SQL Server

W tym samouczku Wybraliśmy **publicznej** dostępu maszyn wirtualnych i **Uwierzytelnianie programu SQL Server**. Te ustawienia konfigurowane automatycznie maszyny wirtualnej do zezwalania na połączenia programu SQL Server z dowolnego klienta w Internecie (przy założeniu, że mają poprawne logowania SQL).

>[AZURE.NOTE] Jeśli nie wybrano publicznej podczas inicjowania obsługi administracyjnej, aby uzyskać dostęp do wystąpienia programu SQL Server w Internecie są wymagane dodatkowe czynności. Aby uzyskać więcej informacji zobacz [Łączenie maszyny wirtualnej do programu SQL Server](virtual-machines-windows-sql-connect.md).

W poniższych sekcjach przedstawiono sposobu łączenia się z wystąpienia programu SQL Server w swojej maszyn wirtualnych z innego komputera w Internecie.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Następne kroki
Aby uzyskać inne informacje o korzystaniu z programu SQL Server w Azure zobacz [Programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md) i [Często zadawane pytania](virtual-machines-windows-sql-server-iaas-faq.md).

Aby uzyskać omówienie wideo programu SQL Server na maszyn wirtualnych Azure Obejrzyj [maszyn wirtualnych Azure jest najlepszą platformą w przypadku programu SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Eksploruj ścieżki szkoleniowe](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) dla programu SQL Server w przypadku Azure maszyn wirtualnych.
