<properties
    pageTitle="Analiza biznesowa programu SQL Server | Microsoft Azure"
    description="W tym temacie używa zasobów utworzone za pomocą modelu Klasyczny wdrożenia i opisano dostępne funkcje Business Intelligence (BI) dla programu SQL Server uruchomiony na Azure wirtualnych maszyn."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>Analizy biznesowej programu SQL Server w środowisku maszyn wirtualnych Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Galeria maszyn wirtualnych usługi Microsoft Azure zawiera obrazy, które zawierają instalacji programu SQL Server. Wersje programu SQL Server, obsługiwane w galerii obrazów są te same pliki instalacji, którą można zainstalować na komputerach w lokalnym i maszyn wirtualnych. Ten temat zawiera podsumowanie funkcji programu SQL Server Business Intelligence (BI) zainstalowanych na obrazy i kroki konfiguracji wymagane po zainicjowano obsługę administracyjną maszyny wirtualnej. W tym temacie opisano wdrożenia obsługiwanych topologii dla funkcje analizy Biznesowej oraz najlepsze rozwiązania.

## <a name="license-considerations"></a>Uwagi dotyczące licencji

Istnieją dwa sposoby licencji programu SQL Server w programie Microsoft Azure środowisku maszyn wirtualnych systemu:

1. Zalety mobilności licencji, które są częścią pakietu Software Assurance. Aby uzyskać więcej informacji zobacz [Mobilności licencji poprzez Software Assurance Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. Zapłacić na godzinę stopa z maszyn wirtualnych Azure z zainstalowanego programu SQL Server. Zobacz sekcję "SQL Server" w [Środowisku maszyn wirtualnych systemu ceny](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Aby uzyskać więcej informacji dotyczących licencjonowania oraz bieżącej stawki zobacz [Często zadawane pytania dotyczące licencjonowania maszyn wirtualnych](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server obrazów dostępnych w Azure wirtualnych galerii komputera

Galeria maszyn wirtualnych usługi Microsoft Azure zawiera kilka obrazów, które zawierają programu Microsoft SQL Server. Oprogramowanie do obrazów maszyn wirtualnych zależy od wersji systemu operacyjnego i wersji programu SQL Server. Na liście obrazy, które są dostępne w galerii Azure maszyn wirtualnych ulega częstym zmianom.

![Obraz SQL azure galerii maszyn wirtualnych](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)

![Programu PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Poniższy skrypt programu PowerShell zwraca listę Azure obrazów, które zawierają "SQL-serwer" w Nazwa_obrazu:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Aby uzyskać więcej informacji o wersji i funkcje obsługiwane przez program SQL Server zobacz:

- [Wersje programu SQL Server](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [Funkcje obsługiwane przez poszczególne wersje programu SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>Funkcje analizy BIZNESOWEJ zainstalowany na obrazy Galeria komputera wirtualnego SQL Server

W poniższej tabeli wymieniono funkcje analizy biznesowej zainstalowanym typowych zdjęcia z galerii maszyn wirtualnych usługi Microsoft Azure dla programu SQL Server"

- Program SQL Server 2016 RC3

- SQL Server 2014 Enterprise z dodatkiem SP1

- SQL Server 2014 Standard z dodatkiem SP1

- SQL Server 2012 z dodatkiem SP2 Enterprise

- SQL Server 2012 z dodatkiem SP2 Standard

|Funkcja BI serwera SQL|Zainstalowanym obrazu z galerii|Notatki|
|---|---|---|
|**Raportowanie trybie natywnym usług**|Tak|Zainstalowany, ale wymaga konfiguracji, w tym adres URL Menedżera raportów. W sekcji [Konfigurowanie usług Reporting Services](#configure-reporting-services).|
|**Usług Reporting Services trybie programu SharePoint**|Brak|Obraz galerii maszyn wirtualnych usługi Microsoft Azure nie zawiera programu SharePoint lub w programie SharePoint pliki instalacji. <sup>1</sup>|
|**Wyszukiwania wielowymiarowych usług Analysis Services i danych (OLAP)**|Tak|Zainstalowaniu i skonfigurowaniu jako domyślnego wystąpienia usług Analysis Services|
|**Tabelarycznym usług Analysis Services**|Brak|Obsługiwane programu SQL Server 2012, 2014 i 2016 obrazów, ale go nie jest zainstalowany domyślnie. Instalowanie innego wystąpienia usług Analysis Services. W tym temacie, zobacz sekcję zainstalować inne usług SQL Server i funkcje.|
|**Analysis Services dodatku PowerPivot dla programu SharePoint**|Brak|Obraz galerii maszyn wirtualnych usługi Microsoft Azure nie zawiera programu SharePoint lub w programie SharePoint pliki instalacji. <sup>1</sup>|

<sup>1</sup> dodatkowe informacje na temat programu SharePoint i Azure maszyn wirtualnych, zobacz [Microsoft Azure architektury dla programu SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) i [Wdrożeniem programu SharePoint w programie Microsoft Azure środowisku maszyn wirtualnych systemu](https://www.microsoft.com/download/details.aspx?id=34598).

![Programu PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Uruchom następujące polecenie programu PowerShell, aby wyświetlić listę zainstalowanych usług, które zawierają "SQL" w polu Nazwa usługi.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Ogólne zalecenia i najważniejsze wskazówki

- Zalecany minimalny rozmiar maszyny wirtualnej w komórce **A3** jest podczas korzystania z programu SQL Server Enterprise Edition. Rozmiar maszyn wirtualnych **A4** jest zalecane w przypadku wdrożeń analizy Biznesowej programu SQL Server Analysis Services i usług Reporting Services.

    Uzyskać informacji na temat bieżący rozmiar maszyn wirtualnych zobacz [Maszyn wirtualnych rozmiarów Azure](virtual-machines-linux-sizes.md).

- Najważniejsze wskazówki dotyczące zarządzania dysku mają być przechowywane dane, dziennika i pliki kopii zapasowej na dyskach niż **C**: i **D**:. Na przykład utworzyć dyski danych **E**: i **F**:.

    - Dysk pamięci podręcznej zasady domyślne dysk **C**: nie jest optymalna do pracy z danymi.

    - **D**: dysk jest dyskiem tymczasowe, która jest używana przede wszystkim dla pliku strony. **D**: dysk nie jest zachowywane i nie są zapisywane w magazynie obiektów blob. Zadania zarządzania, takie jak zmiany rozmiaru maszyn wirtualnych Resetuj **D**: dysk. Zaleca się **nie** używać **D**: dysk plików bazy danych, takie jak tymczasowe.

    Aby uzyskać więcej informacji na temat tworzenia i dołączanie dysków Zobacz [Dołączanie dysku danych maszyn wirtualnych](virtual-machines-windows-classic-attach-disk.md).

- Zatrzymywanie lub odinstalowywanie usług, których nie planujesz używać. Na przykład jeśli maszyny wirtualnej jest używany tylko do usług Reporting Services, zatrzymać lub odinstalowywanie usługi Analysis Services i SQL Server Integration Services. Poniższy obraz przedstawia przykład usług, które są uruchamiane domyślnie.

    ![Usługi programu SQL Server](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)

    >[AZURE.NOTE] Aparat bazy danych programu SQL Server jest wymagana obsługiwane scenariusze BI. Na serwerze pojedynczy topologii maszyn wirtualnych aparat bazy danych musi działać na tym samym maszyn wirtualnych.

    Aby uzyskać więcej informacji, należy zapoznać się z następującymi: [Odinstalowywanie usług Reporting Services](https://msdn.microsoft.com/library/hh479745.aspx) i [Odinstaluj wystąpienie Analysis Services](https://msdn.microsoft.com/library/ms143687.aspx).

- **Windows Update** sprawdzanie nowych aktualizacji"ważne". Obrazy maszyn wirtualnych usługi Microsoft Azure często są odświeżane; Jednak aktualizacje ważne może zostaną udostępnione z **Witryny Windows Update** po ostatniego odświeżenia obraz maszyn wirtualnych.

## <a name="example-deployment-topologies"></a>Przykład wdrożenia topologii

Poniżej przedstawiono przykład wdrożonych maszyn wirtualnych usługi Microsoft Azure za pomocą. Topologii tych diagramach są tylko niektóre możliwe topologii, których można używać z funkcje analizy Biznesowej programu SQL Server i maszyn wirtualnych usługi Microsoft Azure.

### <a name="single-virtual-machine"></a>Pojedynczy maszyn wirtualnych

Analysis Services, usług Reporting Services, aparatu bazy danych programu SQL Server i źródeł danych na jednym komputerze wirtualnych.

![Scenariusz 1 maszyn wirtualnych MSR analizy biznesowej](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Dwa maszyn wirtualnych

- Usług Analysis Services, usług Reporting Services i aparat bazy danych programu SQL Server, na jednym komputerze wirtualnych. To wdrożenie obejmuje baz danych serwera raportów.

- Źródła danych na drugim maszyn wirtualnych. Druga maszyn wirtualnych zawiera aparatu bazy danych programu SQL Server jako źródła danych.

![Scenariusz 2 maszyn wirtualnych iaas analizy biznesowej](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Azure mieszanym — danych w bazie danych Azure SQL

- Usług Analysis Services, usług Reporting Services i aparat bazy danych programu SQL Server, na jednym komputerze wirtualnych. To wdrożenie obejmuje baz danych serwera raportów.

- Źródło danych jest baza danych Azure SQL.

![maszyn wirtualnych scenariusze analizy biznesowej iaas i AzureSQL jako źródło danych](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybrydowe — dane lokalne

- W tym przykładzie wdrożenia usług Analysis Services usług Reporting Services i aparat bazy danych programu SQL Server są uruchamiane na jednym komputerze wirtualnych. Maszyny wirtualnej obsługuje baz danych serwera raportów. Maszyny wirtualnej jest dołączony do domeny lokalnej za pośrednictwem wirtualnych sieci Azure lub innych VPN, tunneling rozwiązanie.

- Źródło danych jest lokalnego.

![maszyn wirtualnych scenariusze iaas analizy biznesowej i w wersji lokalnej źródeł danych](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Konfiguracja trybie natywnym usług raportowania

Obraz galerii maszyn wirtualnych dla programu SQL Server zawiera Reporting Services natywnych tryb zainstalowany, jednak nie jest skonfigurowany na serwerze raportów. Czynności opisane w tej sekcji skonfigurować na serwerze raportów usług Reporting Services. Aby uzyskać bardziej szczegółowe informacje na temat konfigurowania trybie natywnym usług raportowania zobacz [Instalowanie Reporting Services natywnych tryb raportu Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

>[AZURE.NOTE] Podobne zawartości, która używa skrypty środowiska Windows PowerShell w celu skonfigurowania serwera raportów zobacz [Używanie tworzenie Azure maszyn wirtualnych z natywnych tryb serwer raportów](virtual-machines-windows-classic-ps-sql-report.md).

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>Łączenie maszyn wirtualnych i rozpocząć Menedżer konfiguracji Reporting Services

Istnieją dwa typowe przepływów pracy na potrzeby łączenia do maszyn wirtualnych Azure:

- Nawiązywanie połączenia, kliknij nazwę maszyny wirtualnej, a następnie kliknij przycisk **Połącz**. Podłączanie pulpitu zdalnego zostanie otwarty i nazwa komputera jest automatycznie wypełniane.

    ![Nawiązywanie połączenia z azure maszyn wirtualnych](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)

- Połącz maszyn wirtualnych z Podłączanie pulpitu zdalnego w systemie Windows. W interfejsie użytkownika pulpitu zdalnego:

    1. Wpisz **nazwę usługi w chmurze** jako nazwa komputera.

    1. Wpisz numer portu publicznej, który jest skonfigurowany do TCP zdalnego pulpitu punktu końcowego i dwukropek (:).

        Myservice.cloudapp.NET:63133

        Aby uzyskać więcej informacji, zobacz [Co to jest usługa w chmurze?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).

**Rozpocznij Menedżer usług Reporting Services konfigurację.**

1. W **systemie Windows Server 2012**:

1. Na ekranie **startowym** wpisz **Usług Reporting Services** , aby wyświetlić listę aplikacji.

1. Kliknij prawym przyciskiem myszy **Menedżera konfiguracji usług Reporting Services** , a następnie kliknij polecenie **Uruchom jako Administrator**.

1. W **systemie Windows Server 2008 R2**:

1. Kliknij przycisk **Start**, a następnie kliknij pozycję **Wszystkie programy**.

1. Kliknij pozycję **Microsoft SQL Server 2016**.

1. Kliknij pozycję **Narzędzia konfiguracji**.

1. Kliknij prawym przyciskiem myszy **Menedżera konfiguracji usług Reporting Services** , a następnie kliknij polecenie **Uruchom jako Administrator**.

Lub

1. Kliknij przycisk **Start**.

1. W oknie dialogowym **Wyszukaj programy i pliki** wpisz **usługi reporting services**. Jeśli maszyn wirtualnych działa system Windows Server 2012, wpisz **usługi reporting services** na ekranie startowym systemu Windows Server 2012.

1. Kliknij prawym przyciskiem myszy **Menedżera konfiguracji usług Reporting Services** , a następnie kliknij polecenie **Uruchom jako Administrator**.

    ![Wyszukaj menedżera konfiguracji ssrs](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Konfigurowanie usług Reporting Services

**Konto usługi i usług sieci web adres URL:**

1. Upewnij się, że **Nazwa serwera** jest nazwą lokalnego serwera, a następnie kliknij przycisk **Połącz**.

1. Uwaga pustą **Nazwa bazy danych serwera raportów**. Po zakończeniu konfiguracji utworzeniu bazy danych.

1. Sprawdź, czy **Serwer raportów o stanie** jest **uruchomiony**. Jeśli chcesz sprawdzić, czy usługa w systemie Windows Server Manager Usługa jest usługę Windows **Usług SQL Server Reporting Services** .

1. Kliknij **Konto usługi** i zmienić konto, stosownie do potrzeb. Jeśli maszyny wirtualnej jest używany w środowisku połączonych nienależących do domeny, wystarczy wbudowanego konta **serwera raportowania** . Aby uzyskać więcej informacji na konto usługi zobacz [Konta usługi](https://msdn.microsoft.com/library/ms189964.aspx).

1. W lewym okienku kliknij pozycję **Adres URL usługi sieci Web** .

1. Kliknij przycisk **Zastosuj** , aby skonfigurować wartości domyślne.

1. Uwaga **adresy URL usługi sieci Web serwera raportów**. Uwaga domyślny port TCP 80 jest i stanowi część adresu URL. W ostatnim kroku możesz utworzyć punkt końcowy Microsoft Azure maszyn wirtualnych portu.

1. W okienku **wyników** Sprawdź akcje ukończona pomyślnie.

**Funkcje bazy danych**

1. Kliknij **bazę danych** w okienku po lewej stronie.

1. Kliknij pozycję **Zmień bazę danych**.

1. Sprawdź, czy wybrano opcję **Utwórz nową bazę danych serwera raportowania** , a następnie kliknij przycisk Dalej.

1. Sprawdź **Nazwę serwera** , a następnie kliknij przycisk **Testuj połączenie**.

1. Jeśli wynikiem funkcji jest **Testuj połączenie zakończyła się pomyślnie**, kliknij **przycisk OK** , a następnie kliknij przycisk **Dalej**.

1. Uwaga nazwy bazy danych jest **serwera raportowania** i **trybu Serwer raportów** jest **macierzystych** , a następnie kliknij przycisk **Dalej**.

1. Kliknij przycisk **Dalej** na stronie **poświadczenia** .

1. Na stronie **Podsumowanie** kliknij przycisk **Dalej** .

1. Kliknij przycisk **Dalej** na stronie **informacje o postępie i zakończenie** .

**Sieci Web portalu adres URL lub adres URL Menedżera raportów dla 2012 i 2014 r.:**

1. Kliknij **Adres URL portalu sieci Web**lub **Adres URL Menedżera raportów** dla 2014 i 2012, w okienku po lewej stronie.

1. Kliknij przycisk **Zastosuj**.

1. W okienku **wyników** Sprawdź akcje ukończona pomyślnie.

1. Kliknij przycisk **Zakończ**.

Aby uzyskać informacji na temat uprawnień serwera raportów zobacz [Udzielanie uprawnień do natywnej serwer raportów tryb](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Przejdź do lokalnego Menedżera raportów

Aby sprawdzić konfigurację, przejdź do Menedżera raportów na maszyn wirtualnych.

1. Na Głosowa Uruchom program Internet Explorer z uprawnieniami administratora.

1. Przejdź do http://localhost/reports na maszyn wirtualnych.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>Połącz z portalu zdalnego w sieci web lub Menedżera raportów dla 2014 i 2012

Jeśli chcesz połączyć z portalu lub Report Manager 2014 i 2012, na komputerze wirtualnych z komputera zdalnego, Utwórz nowe maszyny wirtualnej punktu końcowego TCP. Domyślnie na serwerze raportów odbiera żądania HTTP na **porcie 80**. Jeśli skonfigurujesz adresy URL serwera raportu, aby użyć innego portu, musisz określić ten numer w poniższych instrukcjach.

1. Tworzenie punktu końcowego dla maszyny wirtualnej z TCP Port 80. Aby uzyskać więcej informacji, zobacz sekcję [punkty końcowe maszyn wirtualnych i portów zapory](#virtual-machine-endpoints-and-firewall-ports) w tym dokumencie.

1. Otwórz port 80 w zaporze maszyny wirtualnej.

1. Przejdź do portalu lub report manager przy użyciu maszyn wirtualnych Azure **Nazwy DNS** jako nazwy serwera w adresie URL. Na przykład:

    **Serwer raportów**: http://uebi.cloudapp.net/reportserver  **portalu**: http://uebi.cloudapp.net/reports

    [Konfigurowanie zapory uzyskać dostęp do serwera raportów](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Tworzenie i publikowanie raportów Azure maszyn wirtualnych

W poniższej tabeli przedstawiono niektóre z opcji dostępnych do publikowania istniejących raportów z komputera lokalnego na serwerze raportów hostowana na Microsoft Azure Virtual Machine:

- **Report Builder**: maszyny wirtualnej obejmuje kliknij-raz wersji programu Microsoft SQL Server Report Builder SQL 2014 i 2012. Aby rozpocząć raportować czas Konstruktor pierwszej na komputerze wirtualnych z SQL 2016:

    1. Uruchom przeglądarkę z uprawnieniami administratora.

    1. Przejdź do portalu sieci web na komputerze wirtualnych i wybierz ikonę **pobierania** w prawym górnym rogu.
    
    1. Wybierz pozycję **Konstruktor raportów**.

    Aby uzyskać więcej informacji zobacz [Rozpoczynanie Report Builder](https://msdn.microsoft.com/library/ms159221.aspx).

- **Program SQL Server Data Tools**: maszyn wirtualnych: SQL Server Data Tools jest zainstalowany na komputerze wirtualnych i służy do tworzenia **Projektów na serwerze raportów** i raportów na komputerze wirtualnych. Program SQL Server Data Tools można opublikować raportów na serwerze raportów na komputerze wirtualnych.

- **SQL Server Data Tools: zdalnego**: na komputerze lokalnym, należy utworzyć projekt usług Reporting Services w SQL Server Data Tools, która zawiera raporty usług Reporting Services. Konfigurowanie projektu, aby nawiązać połączenie adres URL usługi sieci web.

    ![Właściwości projektu SSDT SSRS projektu](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)

- Tworzenie. Wirtualnego dysku twardego dysku twardym, który zawiera raporty, a następnie przekaż i dodać dysk.

    1. Tworzenie. Dysk twardy wirtualnego dysku twardego na komputerze lokalnym, która zawiera raportów.

    1. Utwórz i zainstaluj certyfikat zarządzania.

    1. Przekaż plik wirtualnego dysku twardego Azure za pomocą polecenia cmdlet Dodaj AzureVHD [Tworzenie i przekazywanie Windows serwera wirtualnego dysku twardego Azure](virtual-machines-windows-classic-createupload-vhd.md).

    1. Dołącz dysk maszyn wirtualnych.

## <a name="install-other-sql-server-services-and-features"></a>Instalowanie innych usług SQL Server i funkcje

Aby zainstalować dodatkowe usługi programu SQL Server, takie jak usług Analysis Services w trybie tabelarycznym, uruchom Kreator konfiguracji programu SQL server. Pliki konfiguracji znajdują się na lokalnym dysku maszyny wirtualnej.

1. Kliknij przycisk **Start** , a następnie kliknij pozycję **Wszystkie programy**.

1. Kliknij pozycję **Microsoft SQL Server 2016**, **program Microsoft SQL Server 2014** lub **programu Microsoft SQL Server 2012** , a następnie kliknij pozycję **Narzędzia konfiguracji**.

1. Kliknij pozycję **Centrum instalacji programu SQL Server**.

Lub uruchamianie C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe lub C:\SQLServer_11.0_full\setup.exe

>[AZURE.NOTE] Podczas pierwszego uruchomienia instalacji programu SQL Server więcej plików konfiguracji można pobrać i wymaga ponownego uruchomienia maszyny wirtualnej i ponowne uruchomienie instalacji programu SQL Server.
>
>Jeśli trzeba wielokrotnie Dostosowywanie obraz z Microsoft Azure Virtual Machine, rozważ utworzenie własnego obrazu programu SQL Server. Funkcje narzędzia SysPrep usług Analysis zostało włączone z programu SQL Server 2012 z dodatkiem SP1 CU2. Aby uzyskać więcej informacji zobacz [informacje dotyczące instalacji programu SQL Server przy użyciu narzędzia SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) i [Obsługa narzędzia Sysprep dla ról serwera](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="to-install-analysis-services-tabular-mode"></a>Aby zainstalować tryb tabelaryczny Analysis Services

Kroki w tej sekcji, **podsumowywanie** instalacji tryb tabelarycznym usług Analysis Services. Aby uzyskać więcej informacji zobacz:

- [Instalowanie usług Analysis Services w trybie tabelaryczny](https://msdn.microsoft.com/library/hh231722.aspx)

- [Modelowania tabelarycznego (Adventure Works samouczek)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Aby zainstalować tryb tabelarycznym usług Analysis:**

1. W Kreatorze instalacji programu SQL Server, kliknij opcję **Instalacja** w okienku po lewej stronie, a następnie kliknij pozycję **Instalacja autonomiczny nowego programu SQL server lub dodawanie funkcji do istniejącej instalacji**.

    - Jeśli widzisz **Przeglądanie w poszukiwaniu folderu**, przejdź do c:\SQLServer_13.0_full, c:\SQLServer_12.0_full lub c:\SQLServer_11.0_full, a następnie kliknij przycisk **Ok**.

1. Kliknij przycisk **Dalej** na stronie Aktualizacje produktów.

1. Na stronie **Typ instalacji** wybierz pozycję **Wykonaj nowej instalacji programu SQL Server** , a następnie kliknij przycisk **Dalej**.

1. Na stronie **Ustawienia roli** kliknij **Instalacja funkcji programu SQL Server**.

1. Na stronie **Wybór funkcji** kliknij **Usług Analysis Services**.

1. Na stronie **Konfiguracja wystąpienia** wpisz opisową nazwę, na przykład **Tabelaryczny** polach tekstowych **o nazwie wystąpienia** i **Identyfikator wystąpienia** .

1. Na stronie **Konfiguracja usługi Analysis Services** wybierz **Tryb tabelaryczny**. Dodaj bieżącego użytkownika do listy uprawnień administracyjnych.

1. Wykonywanie i zamknij Kreatora instalacji programu SQL Server.

## <a name="analysis-services-configuration"></a>Konfiguracja usługi Analysis Services

### <a name="remote-access-to-analysis-services-server"></a>Dostęp zdalny do serwer usług Analysis Services

Serwer usług Analysis Services obsługuje tylko uwierzytelnianie systemu windows. Aby uzyskać dostęp do usług Analysis Services zdalnie z aplikacji klienta, takich jak program SQL Server Management Studio lub SQL Server Data Tools, maszyny wirtualnej musi być dołączane do domeny lokalnej za pomocą Azure wirtualne sieci. Aby uzyskać więcej informacji, zobacz [Azure wirtualnej sieci](../virtual-network/virtual-networks-overview.md).

W przypadku **wystąpienia domyślnego** usług Analysis Services wykrywa na port TCP **2383**. Otwórz port w zaporze maszyn wirtualnych. Grupowany nazwanego wystąpienia usług Analysis Services również wykrywa na port **2383**.

Dla **o nazwie wystąpienia** usług Analysis Services usługa Przeglądarka SQL Server jest wymagane do zarządzania do portu. Konfiguracja domyślna przeglądarka SQL Server jest port **2382**.

W środowisku maszyn wirtualnych systemu Zapora Otwórz port **2382** i Utwórz statyczne Analysis Services o nazwie port wystąpienia.

1. Aby sprawdzić portów, które są już używane na maszyn wirtualnych i jakie proces korzysta z portów, uruchom następujące polecenie z uprawnieniami administratora:

        netstat /ao

1. Użyj SQL Server Management Studio, aby utworzyć statyczne usług Analysis Services o nazwie port wystąpienia aktualizując "Port" wartość w tabeli jako wystąpienia właściwości ogólne. Aby uzyskać więcej informacji zobacz "używania stałego portu domyślnym lub nazwane wystąpienie" w [artykule Konfigurowanie Zapory systemu Windows, aby umożliwić dostępu do usług Analysis](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).

1. Ponownie uruchom wystąpienie tabelarycznym usług Analysis Services.

Aby uzyskać więcej informacji, zobacz sekcję **punkty końcowe maszyn wirtualnych i portów zapory** w tym dokumencie.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Punkty końcowe maszyn wirtualnych i portów zapory

W tej sekcji przedstawiono Microsoft Azure maszyn wirtualnych końcowymi do tworzenia i porty do otwarcia w zaporze maszyn wirtualnych. W poniższej tabeli podsumowano porty **TCP** , aby utworzyć punktów końcowych i porty do otwarcia w zaporze maszyn wirtualnych.

- Jeśli korzystasz z jednego maszyn wirtualnych i dwa elementy są spełnione, nie musisz tworzyć punkty końcowe maszyn wirtualnych i nie musisz otworzyć porty w zaporze na maszyn wirtualnych.

    - Funkcje programu SQL Server na maszyn wirtualnych nie zdalnie nawiązać. Podłączanie pulpitu zdalnego do maszyn wirtualnych ustanawiania i uzyskiwanie dostępu do funkcji programu SQL Server lokalnie na maszyn wirtualnych nie są uwzględniane połączenia zdalnego z funkcji programu SQL Server.

    - Nie dołączaj maszyn wirtualnych do domeny lokalnej za pośrednictwem wirtualnych sieci Azure lub inne rozwiązanie tunelowania VPN.

- Jeśli maszyna wirtualna nie jest dołączony do domeny, ale chcesz zdalnie łączyć się z funkcji programu SQL Server na maszyn wirtualnych:

    - Otwórz porty w zaporze na maszyn wirtualnych.

    - Tworzenie maszyn wirtualnych punkty końcowe uwagę na wspomniane portów (*).

- Jeśli maszyna wirtualna jest dołączony do domeny za pomocą tunelem VPN, takie jak wirtualna sieć Azure, punkty końcowe nie są wymagane. Jednak Otwórz porty w zaporze na maszyn wirtualnych.

  	|Port|Typ|Opis|
|---|---|---|
|**80**|PORT TCP|Serwer raportów dostępu zdalnego (*).|
|**1433**|PORT TCP|SQL Server Management Studio (*).|
|**1434**|UDP|Przeglądarka SQL Server. Jest to potrzebne, gdy maszyn wirtualnych w dołączony do domeny.|
|**2382**|PORT TCP|Przeglądarka SQL Server.|
|**2383**|PORT TCP|Wystąpienia domyślnego usług SQL Server Analysis Services i grupowany nazwanych wystąpień.|
|**Zdefiniowane przez użytkownika**|PORT TCP|Tworzenie statycznej Analysis Services o nazwie wystąpienia port numer portu, który możesz wybrać, a następnie odblokować numer portu w zaporze.|

Aby uzyskać więcej informacji na temat tworzenia punktów końcowych zobacz:

- Tworzenie punkty końcowe:[jak skonfigurować punkty końcowe maszyn wirtualnych](virtual-machines-windows-classic-setup-endpoints.md).

- Program SQL Server: Zobacz sekcję "Konfiguracji wykonano kroki, aby połączyć się z komputerem wirtualnych używanie SQL Server Management Studio" [inicjowania obsługi administracyjnej maszyny wirtualnej serwera SQL Azure](virtual-machines-windows-portal-sql-server-provision.md).

Na poniższym diagramie przedstawiono porty do otwarcia w zaporze maszyn wirtualnych, aby umożliwić dostęp zdalny do funkcji i składników na maszyn wirtualnych.

![porty do otwarcia aplikacji biznesowej w maszyny wirtualne Azure](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Zasoby

- Przejrzyj zasady pomocy technicznej dla oprogramowania serwera Microsoft używany w środowisku maszyn wirtualnych Azure. Poniższy temat zawiera podsumowanie Obsługa funkcji, takich jak BitLocker awaryjnej lub równoważenia obciążenia sieciowego. [Oprogramowanie serwera Microsoft pomocy technicznej dla maszyn wirtualnych Azure](http://support.microsoft.com/kb/2721672).

- [Program SQL Server na podglądzie Azure maszyn wirtualnych](virtual-machines-windows-sql-server-iaas-overview.md)

- [Maszyn wirtualnych](https://azure.microsoft.com/documentation/services/virtual-machines/)

- [Obsługa administracyjna maszyny wirtualnej serwera SQL Azure](virtual-machines-windows-portal-sql-server-provision.md)

- [Jak dołączyć dysku danych maszyn wirtualnych](virtual-machines-windows-classic-attach-disk.md)

- [Migrowanie bazy danych do programu SQL Server na Azure maszyn wirtualnych](virtual-machines-windows-migrate-sql.md)

- [Określanie tryb serwera wystąpienia usług Analysis](https://msdn.microsoft.com/library/gg471594.aspx)

- [Modelowanie wielowymiarowych (Adventure Works samouczek)](https://technet.microsoft.com/library/ms170208.aspx)

- [Centrum dokumentacji Azure](https://azure.microsoft.com/documentation/)

- [Za pomocą usługi Power BI w środowisku hybrydowym](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [Przesyłanie opinii i informacje kontaktowe za pośrednictwem połączenia programu Microsoft SQL Server](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Zawartość społeczności

- [Zarządzanie bazą danych Azure SQL za pomocą programu PowerShell](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)
