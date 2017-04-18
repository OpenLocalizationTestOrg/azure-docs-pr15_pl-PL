<properties
    pageTitle="Omówienie programu SQL Server w przypadku Azure maszyn wirtualnych | Microsoft Azure"
    description="Informacje na temat sposobu uruchamiania pełny wersje programu SQL Server na komputerach wirtualnych Azure. Uzyskaj bezpośrednie łącza do wszystkich obrazów maszyn wirtualnych programu SQL Server i zawartości pokrewnej."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Omówienie programu SQL Server w przypadku Azure maszyn wirtualnych

W tym temacie opisano opcje klastrze programu SQL Server Azure wirtualnych maszyn, oraz [łącza do portalu obrazów](#option-1-create-a-sql-vm-with-per-minute-licensing) oraz omówienie [typowych zadań](#manage-your-sql-vm).

>[AZURE.NOTE] Jeśli znasz już program SQL Server i chcesz jedynie wyświetlić jak wdrożyć maszyn wirtualnych programu SQL Server, zobacz [Obsługa administracyjna maszyny wirtualnej programu SQL Server w portalu Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Omówienie
Jeśli jesteś administratorem bazy danych lub deweloper Azure maszyny wirtualne umożliwiają przechodzenie do lokalnego programu SQL Server obciążenia i aplikacje w chmurze. Poniższym klipie wideo przedstawiono informacje techniczne dotyczące maszyny wirtualne programu SQL Server Azure.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Klip wideo omówiono następujące zagadnienia:

|Czas|Obszar|
|---|---|
| 00:21 | Co to są maszyny wirtualne Azure? |
| 01:45 | Zabezpieczenia |
| 02:50 | Łączność |
| 03:30 | Miejsca do magazynowania niezawodności i wydajności |
| 05:20 | Rozmiary maszyn wirtualnych |
| 05:54 | Umowa dotycząca poziomu usług i wysokiej dostępności |
| 07:30 | Obsługa konfiguracji |
| 08:00 | Monitorowanie |
| 08:32 | Pokaz: Tworzenie maszyn wirtualnych SQL Server 2016 |

>[AZURE.NOTE] Klip wideo omówiono SQL Server 2016, ale Azure zapewnia obrazów maszyn wirtualnych dla wielu wersji programu SQL Server: 2008, 2012 2014 i 2016. 

## <a name="scenarios"></a>Scenariusze
Istnieje wiele powodów, dla których warto wybrać do przechowywania danych w Azure. Jeśli aplikacja jest przenoszona do Azure, zwiększa wydajność również przenieść dane. Istnieją inne korzyści. Automatycznie mieć dostęp do wielu centrach danych globalnej odzyskiwania informacje o obecności i awarii. Dane są również bardzo zabezpieczone i trwałe.

Program SQL Server uruchomionych maszyny wirtualne Azure jest jednym ze do przechowywania danych relacyjnych w Azure. Jest dobrym rozwiązaniem dla kilka scenariuszy. Na przykład można skonfigurować maszyn wirtualnych Azure podobnie jak najbliżej maszyny do lokalnego programu SQL Server. Możesz też uruchomić dodatkowe aplikacje i usługi na tym samym serwerze bazy danych. Istnieją dwa główne zasoby, które ułatwiają analizę jeszcze więcej scenariuszy i zagadnienia:

 - [Program SQL Server w przypadku Azure maszyn wirtualnych](https://azure.microsoft.com/services/virtual-machines/sql-server/) omówiono najważniejsze scenariusze dotyczące korzystania z programu SQL Server w maszyny wirtualne Azure. 
 - [Wybierz chmury opcja programu SQL Server: bazy danych SQL Azure (PaaS) lub SQL Server na Azure maszyny wirtualne (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) umożliwia szczegółowe porównanie bazy danych SQL i SQL Server uruchomionych maszyny.

## <a name="create-a-new-sql-vm"></a>Tworzenie nowej maszyny SQL
W poniższych sekcjach przedstawiono bezpośrednie łącza do portalu Azure galerii obrazów maszyn wirtualnych programu SQL Server. W zależności od wybranego obrazu można albo płatności dla programu SQL Server Licencjonowanie kosztów na minutę lub możesz zabrać ze sobą własnych licencji (BYOL).

Znajdź instrukcje krok po kroku dla tego procesu w samouczku [świadczenia maszyny wirtualnej programu SQL Server w portalu Azure](virtual-machines-windows-portal-sql-server-provision.md). Ponadto przejrzyj [wydajności najważniejsze wskazówki dotyczące maszyny wirtualne programu SQL Server](virtual-machines-windows-sql-performance.md), której wyjaśniono, jak zaznaczyć odpowiednią rozmiarów i inne funkcje dostępne podczas inicjowania obsługi administracyjnej.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Opcja 1: Tworzenie maszyny SQL z licencjonowania na minutę
Poniższa tabela zawiera macierzy dostępnych obrazów programu SQL Server w galerii maszyn wirtualnych. Kliknij dowolne łącze, aby rozpocząć tworzenie nowej maszyny SQL z określonej wersji, edition i systemu operacyjnego.

|Wersja|System operacyjny|Edition|
|---|---|---|
|**SQL 2016**|Windows Server 2012 R2|[Przedsiębiorstwo](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standardowy](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [sieci Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [deweloperów](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**Z DODATKIEM SP1 SQL 2014 R.**|Windows Server 2012 R2|[Przedsiębiorstwo](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standardowy](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [sieci Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014 R.**|Windows Server 2012 R2|[Przedsiębiorstwo](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standardowy](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [sieci Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 Z DODATKIEM SP3**|Windows Server 2012 R2|[Przedsiębiorstwo](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standardowy](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [sieci Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 Z DODATKIEM SP2**|Windows Server 2012 R2|[Przedsiębiorstwo](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standardowy](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [sieci Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 Z DODATKIEM SP2**|System Windows Server 2012|[Przedsiębiorstwo](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standardowy](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [sieci Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 Z DODATKIEM SP3**|Windows Server 2008 R2|[Przedsiębiorstwo](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standardowy](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [sieci Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 Z DODATKIEM SP3**|System Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Opcja 2: Tworzenie maszyny SQL z istniejących licencji
Możesz również zabrać ze sobą własnych licencji (BYOL). W tym scenariuszu tylko płatność dla maszyn wirtualnych bez wszelkie dodatkowe opłaty licencjonowania programu SQL Server. Aby użyć własnego licencji, użyj macierzy wersje programu SQL Server, wersje i systemy operacyjne poniżej. W portalu te nazwy obrazu są prefiksem **{BYOL}**.

|Wersja|System operacyjny|Edition|
|---|---|---|
|**Program SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [BYOL standardowy](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**Program SQL Server 2014 z dodatkiem SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [BYOL standardowy](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**Program SQL Server 2012 z dodatkiem SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [BYOL standardowy](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Aby użyć obrazów maszyn wirtualnych BYOL, musisz mieć oraz Enterprise Agreement z [Mobilności licencji poprzez Software Assurance Azure](https://azure.microsoft.com/pricing/license-mobility/). Należy również prawidłową licencję wersji i wydania programu SQL Server, którego chcesz użyć. W ciągu **10** dni od inicjowania obsługi administracyjnej usługi maszyn wirtualnych należy [podać wymagane informacje BYOL do firmy Microsoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) .

## <a name="manage-your-sql-vm"></a>Zarządzanie maszyn wirtualnych usługi SQL
Po zainicjowaniu obsługi administracyjnej maszyn wirtualnych usługi SQL Server, istnieje kilka zadań związanych z zarządzaniem opcjonalne. W wielu aspektach, konfigurowanie i zarządzania programu SQL Server, dokładnie tak, jak chcesz zarządzać wystąpienie programu SQL Server lokalnego. Jednak niektóre zadania są specyficzne dla Azure. Poniższe sekcje wyróżnić niektóre z tych obszarów z łącza do dodatkowych informacji.

### <a name="connect-to-the-vm"></a>Nawiązywanie połączenia z maszyn wirtualnych
Jedną z najbardziej podstawowych zadań zarządzania jest nawiązać maszyn wirtualnych usługi SQL Server za pomocą narzędzi, takich jak program SQL Server Management Studio (SSMS). Aby uzyskać instrukcje dotyczące sposobu łączenia się z nowych maszyn wirtualnych serwera SQL zobacz [Łączenie z programu SQL Server maszyny wirtualnej Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrowanie danych
Jeśli masz istniejącej bazy danych chcesz przenieść do nowo ustanawianie maszyn wirtualnych SQL. Aby uzyskać listę opcji migracji i wskazówki zobacz [Migrowanie bazy danych programu SQL Server na maszyn wirtualnych Azure](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Konfigurowanie wysokiej dostępności
Jeśli potrzebna jest wysoka dostępność, należy rozważyć skonfigurowanie grupy dostępności programu SQL Server. Ten proces obejmuje wiele Azure maszyny wirtualne w wirtualnej sieci. Azure portal zawiera szablon, który ustawia tej konfiguracji dla Ciebie. Aby uzyskać więcej informacji zobacz [Konfigurowanie grupy dostępności (AlwaysOn) w środowisku maszyn wirtualnych Azure Menedżera zasobów](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Jeśli chcesz ręcznie skonfiguruj grupy dostępności i skojarzone odbiornika, zobacz [Konfigurowanie grupy dostępności (AlwaysOn) w Azure maszyn wirtualnych](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Inne zagadnienia szybkiej uzyskać [wysoką dostępność i odzyskiwanie w przypadku programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Tworzenie kopii zapasowych danych
Azure maszyny wirtualne można korzystać z [Automatycznego kopii zapasowej](virtual-machines-windows-sql-automated-backup.md), który regularnie tworzy kopie zapasowe bazy danych z magazynem obiektów blob. Można też ręcznie Użyj tej techniki. Aby uzyskać więcej informacji zobacz [Używanie Azure miejsca do magazynowania dla i przywracania kopii zapasowych programu SQL Server](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Aby uzyskać omówienie wszystkich opcji kopia zapasowa i przywracanie zobacz [Kopia zapasowa i przywracanie dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatyzowanie aktualizacji
Azure maszyny wirtualne umożliwia [Automatyczne poprawianie](virtual-machines-windows-sql-automated-patching.md) planowanie oknem konserwacji do instalowania ważnych systemu windows i automatycznie aktualizuje programu SQL Server.

### <a name="customer-experience-improvement-program-ceip"></a>Program poprawy jakości obsługi klienta (CEIP)
Program poprawy jakości obsługi klienta (CEIP) jest domyślnie włączona. Raporty zostanie okresowo wysyła do firmy Microsoft w celu zwiększenia programu SQL Server. Nie ma żadnych zadań zarządzania wymagane z poprawy jakości obsługi klienta, chyba że chcesz wyłączyć, po zainicjowaniu obsługi administracyjnej. Można dostosować lub wyłączyć poprawy jakości obsługi klienta przez nawiązanie połączenia z maszyn wirtualnych za pomocą pulpitu zdalnego. Następnie uruchom narzędzie **błąd serwera SQL i raportowania użycia** . Postępuj zgodnie z instrukcjami, aby wyłączyć raportowanie. 

Aby uzyskać więcej informacji zobacz sekcję poprawy jakości obsługi klienta tematu [Zaakceptuj postanowienia licencyjne](https://msdn.microsoft.com/library/ms143343.aspx) . 

## <a name="next-steps"></a>Następne kroki
[Eksploruj ścieżki szkoleniowe](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) dla programu SQL Server w przypadku Azure maszyn wirtualnych.

Więcej pytanie? Najpierw zobacz [Programu SQL Server w środowisku maszyn wirtualnych systemu Azure — często zadawane pytania](virtual-machines-windows-sql-server-iaas-faq.md). Ale również dodać usługi pytania i komentarze u dołu wszystkich tematów maszyn wirtualnych SQL współdziałanie z firmy Microsoft i społeczności.
