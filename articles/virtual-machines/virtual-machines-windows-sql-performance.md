<properties
    pageTitle="Wydajność najważniejsze wskazówki dotyczące programu SQL Server | Microsoft Azure"
    description="Zawiera najważniejsze wskazówki dotyczące optymalizowania wydajności programu SQL Server w pośrednictwem Microsoft Azure SMS."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/07/2016"
    ms.author="jroth" />

# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Wydajność najważniejsze wskazówki dotyczące programu SQL Server w maszyn wirtualnych Azure

## <a name="overview"></a>Omówienie

Ten temat zawiera wskazówki dotyczące optymalizowania wydajności programu SQL Server w programie Microsoft Azure Virtual Machine. Podczas uruchamiania programu SQL Server w maszyn wirtualnych Azure, zalecamy kontynuowanie przy użyciu samej wydajności bazy danych, dostosowywanie opcji, które mają zastosowanie do programu SQL Server w środowisku serwera lokalnego. Jednak wydajność relacyjnej bazy danych w chmurze publicznej zależy od wiele czynników, takich jak rozmiar maszyny wirtualnej i konfigurację dyski danych.

Podczas tworzenia obrazów programu SQL Server, [należy rozważyć, czy inicjowania obsługi administracyjnej usługi maszyny wirtualne w portalu Azure](virtual-machines-windows-portal-sql-server-provision.md). Maszyny wirtualne serwera SQL zainicjowano obsługę administracyjną w portalu przy użyciu Menedżera zasobów wykonania wszystkich tych najważniejszych wskazówek, w tym konfigurację miejsca do magazynowania.

Ten artykuł dotyczy wprowadzenie *najlepszą* wydajność dla programu SQL Server na maszyny wirtualne Azure. W przypadku usługi Obciążenie pracą mniej wymagające, nie może wymagać każdej optymalizacji wymienione poniżej. Należy rozważyć potrzeby wydajności i desenie obciążenie pracą, jak oceny tych zaleceń.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Szybkie sprawdzanie listy

Oto lista szybkie sprawdzenie umożliwiające uzyskanie optymalnej wydajności programu SQL Server na maszyn wirtualnych Azure:

|Obszar|Optymalizacja|
|---|---|
|[Rozmiar pamięci Wirtualnej](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) lub nowszy dla SQL Enterprise edition.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) lub nowszej wersji SQL Standard i sieci Web.|
|[Miejsca do magazynowania](#storage-guidance)|Za pomocą [Premium miejsca do magazynowania](../storage/storage-premium-storage.md). Zaleca się tylko standardowe miejsca do magazynowania dla deweloperów i przetestuj.<br/><br/>Zachowaj [konta miejsca do magazynowania](../storage/storage-create-storage-account.md) i maszyn wirtualnych programu SQL Server w tym samym regionie.<br/><br/>Wyłącz Azure [zbędne geo przestrzeni dyskowej](../storage/storage-redundancy.md) (geo replikacji) na koncie miejsca do magazynowania.|
|[Dysków](#disks-guidance)|Użyj co najmniej 2 [dysków P30](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) (1 dla plików dziennika; 1 dla plików danych i tymczasowe).<br/><br/>Unikaj używania systemu operacyjnego lub tymczasowe dysków do przechowywania bazy danych lub rejestrowanie.<br/><br/>Włącz przeczytaj pamięci podręcznej na dyskach hostingu pliki danych i tymczasowe.<br/><br/>Nie należy włączać pamięci podręcznej na dyskach hostingu pliku dziennika.<br/><br/>Ważne: Zatrzymywanie usługi SQL Server, podczas zmieniania ustawień pamięci podręcznej dysku maszyn wirtualnych Azure.<br/><br/>Paskowych wiele dysków Azure danych, aby uzyskać lepszą wydajność Jo.<br/><br/>Format z opisano rozmiarów alokacji.|
|[WE/WY](#io-guidance)|Włączanie kompresji strony bazy danych.<br/><br/>Włącz inicjowanie błyskawiczne pliku dla plików danych.<br/><br/>Ograniczanie lub wyłączanie pomieścić w bazie danych.<br/><br/>Wyłącz autoshrink w bazie danych.<br/><br/>Przenoszenie wszystkich baz danych na dyskach danych, w tym z baz danych systemu.<br/><br/>Przenoszenie programu SQL Server błędu dziennika i śledzenia pliku katalogów na dyskach danych.<br/><br/>Ustawienia domyślnej lokalizacji pliku kopii zapasowej i bazy danych.<br/><br/>Włączanie zablokowanych stron.<br/><br/>Stosowanie poprawki wydajności programu SQL Server.|
|[Funkcja unikatowe](#feature-specific-guidance)|Wykonywanie kopii zapasowej bezpośrednio z magazynem obiektów blob.|

Aby uzyskać więcej informacji na *jak* i *Dlaczego* nawiązać Optymalizacje te Przejrzyj szczegóły i wskazówki opisane w poniższych sekcjach.

## <a name="vm-size-guidance"></a>Wskazówki dotyczące rozmiaru maszyn wirtualnych

Aplikacje poufnych wydajności zaleca się użycie następujących [rozmiarów maszyn wirtualnych](virtual-machines-windows-sizes.md):

- **SQL Server Enterprise Edition**: DS3 lub nowszy

- **Program SQL Server Standard i Web Edition**: DS2 lub nowszy


## <a name="storage-guidance"></a>Wskazówki dotyczące miejsca do magazynowania

Obsługa maszyny wirtualne Zasadami serii (wraz z serii DSv2 i serii GS) [Premium miejsca do magazynowania](../storage/storage-premium-storage.md). Magazyn Premium jest zalecane dla wszystkich obciążenia produkcji.

> [AZURE.WARNING] Magazyn standardowe o różnych szerokościach opóźnienia i przepustowość i jest zalecane tylko dla deweloperów/testowanie obciążenia. Obciążenia produkcji należy używać Premium miejsca do magazynowania.

Ponadto zaleca się utworzenie konta magazynu platformy Azure w tym samym centrum danych jako maszyn wirtualnych programu SQL Server w celu zmniejszenia opóźnień przełączania. Podczas tworzenia konta miejsca do magazynowania, wyłącz replikacji geo kolejności spójne zapisu na wielu dyskach nie jest gwarantowana. Zamiast tego należy rozważyć skonfigurowanie technologia odzyskiwania danych programu SQL Server między dwoma Azure danych. Aby uzyskać więcej informacji zobacz [wysokiej dostępności i odzyskiwanie w przypadku programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="disks-guidance"></a>Wskazówki dotyczące dysków

Istnieją trzy typy głównym dysku na maszyn wirtualnych Azure:

- **Dysk systemu operacyjnego**: podczas tworzenia maszyn wirtualnych Azure platformie wstawi co najmniej jednego dysku (oznaczony jako dysk **C** ) do maszyn wirtualnych dla dysku systemu operacyjnego. Dysk jest przechowywana jako obiektu blob strony w magazynie wirtualnego dysku twardego.
- **Tymczasowe dysku**: maszyn wirtualnych Azure zawierają inny dysk o nazwie dysku tymczasowym (oznaczony jako **D**: dysk). Jest to dysk w węźle, który może być używany dla miejsce zapasowe.
- **Dyski danych**: Możesz również dołączyć dodatkowe dyski na komputerze wirtualnych, jako dyski danych i będą one przechowywane w magazynie jako blob strony.

W poniższych sekcjach opisano zalecenia dotyczące korzystania z tych różnych dysków.

### <a name="operating-system-disk"></a>System operacyjny dysku

Dysk systemu operacyjnego jest wirtualny dysk twardy, który można uruchomić i zainstalować jako uruchomionej wersji systemu operacyjnego i jest oznaczony jako dysk **C** .

Domyślne pamięci podręcznej zasad na dysku systemu operacyjnego jest **Odczytu/zapisu**. Dla aplikacji poufnych wydajności zalecamy Użyj dyski danych zamiast dysku systemu operacyjnego. Zobacz sekcję na dyskach danych poniżej.

### <a name="temporary-disk"></a>Tymczasowe dysku

Dysk tymczasowego przechowywania, oznaczony jako **D**: dysku, nie jest zachowywane z magazynem obiektów blob platformy Azure. Nie należy przechowywać plików bazy danych użytkownika lub pliki dziennika transakcji użytkownika na **D**: dysk.

Dla serii D, Dv2 serii i maszyny wirtualne serii G tymczasowe dysk na te maszyny wirtualne jest oparte na SSD. Jeśli z pracą jest używanych tymczasowe (np tymczasowych obiektów lub złożonych sprzężenia), przechowywania tymczasowe na dysku **D** może spowodować wyższej przepustowości tymczasowe i mniejsze opóźnienia tymczasowe.

Dla maszyny wirtualne obsługujące Premium miejsca do magazynowania (DS serii, DSv2 serii i serii GS) zaleca się przechowywanie tymczasowe i/lub rozszerzenia puli buforu na dysku, który pozwala na przechowywanie Premium z włączone buforowanie odczytu. Istnieje jeden wyjątek do tego zalecenia; do zastosowania tymczasowe w przypadku zapisu intensywną, możesz uzyskać większą wydajność tymczasowe są przechowywane na lokalnym dysku **D** , który jest również według rozmiary maszynowego SSD.

### <a name="data-disks"></a>Dyski danych

- **Za pomocą dysków danych dla plików danych i dziennika**: co najmniej za pomocą 2 Premium magazynu [dysków P30](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) miejsce, w którym jeden dysk zawiera pliki dziennika, a drugi pliki danych i tymczasowe.

- **Rozkładanie**: więcej przepustowości, można dodać dodatkowe dane dysków i używać rozkładanie. Aby określić liczbę dysków danych, konieczne jest analizowanie liczbę operacji i/o na SEKUNDĘ dostępne dla Twojej dyskach danych i dziennika. Te informacje dla tabel w operacji i/o na SEKUNDĘ na [rozmiar pamięci Wirtualnej](virtual-machines-windows-sizes.md) i dysk rozmiar w następujący artykuł: [Przy użyciu magazynu Premium dysków](../storage/storage-premium-storage.md). Za pomocą następujących tych wskazówek:

    - Dla systemu Windows 8/Windows Server 2012 lub nowszego należy użyć [Spacji miejsca do magazynowania](https://technet.microsoft.com/library/hh831739.aspx). Ustaw rozmiar pasek 64 KB dla obciążenia OLTP i 256 KB dla obciążenia magazynowanie danych, aby uniknąć wpływ na wydajność ze względu na niezgodność partycją. Ponadto, ustaw liczbę kolumn = Liczba dysków fizycznych. Aby skonfigurować ilości miejsca do magazynowania z więcej niż 8 dysków musi jawnie ustaw liczbę kolumn w celu dopasowania do liczby dysków za pomocą programu PowerShell (nie Menedżer serwera UI). Aby uzyskać więcej informacji na temat konfigurowania [Spacje miejsca do magazynowania](https://technet.microsoft.com/library/hh831739.aspx), zobacz [Polecenia cmdlet spacje miejsca do magazynowania w programie Windows PowerShell](https://technet.microsoft.com/library/jj851254.aspx)

    - Dla Windows 2008 R2 lub starszy można używać dysków dynamicznych (OS rozłożone objętości), a rozmiar pasek jest zawsze 64 KB. Należy zauważyć, że ta opcja jest wycofane systemu Windows 8/Windows Server 2012. Aby uzyskać informacje Zobacz instrukcję obsługi u [Usługa dysków wirtualnych przechodzi do interfejsu API zarządzania magazynu systemu Windows](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

    - Jeśli obciążenia systemu nie jest intensywnie dziennika i nie ma potrzeby dedykowane operacji i/o na sekundę, można skonfigurować tylko jedną puli miejsca do magazynowania. W przeciwnym razie tworzenie dwóch puli miejsca do magazynowania, jedną dla pliki dziennika i innej puli miejsca do magazynowania dla plików danych i tymczasowe. Określenie liczby dysków związanych z każdego puli miejsca do magazynowania oparte na Twoich oczekiwań ładowania. Należy pamiętać, że różne rozmiary maszyn wirtualnych pozwala różną liczbę dysków załączonych danych. Aby uzyskać więcej informacji zobacz [rozmiarów maszyn wirtualnych](virtual-machines-windows-sizes.md).

    - Jeśli nie używasz Premium magazynowania (scenariuszy deweloperów/testowania), zaleca się dodawanie maksymalną liczbę dysków danych obsługiwanych przez usługi [rozmiar pamięci Wirtualnej](virtual-machines-windows-sizes.md) i używanie rozkładanie.

- **Buforowanie zasad**: dyski danych do magazynowania Premium, Włącz buforowanie odczytu na dyskach danych hostingu tylko pliki danych i tymczasowe. Jeśli nie używasz Premium miejsca do magazynowania, nie należy włączać wszelkie buforowanie na wszelkie dyski danych. Aby uzyskać instrukcje dotyczące konfigurowania buforowania dysku, zobacz następujące tematy: [Ustawianie AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) i [AzureDataDisk zestawu](https://msdn.microsoft.com/library/azure/jj152851.aspx).

    >[AZURE.WARNING] Zatrzymywanie usługi SQL Server, zmieniając ustawienie pamięci podręcznej dysków maszyn wirtualnych Azure, aby uniknąć możliwości uszkodzenie bazy danych.

- **Rozmiar jednostki alokacji NTFS**: podczas formatowania dysku danych, zaleca się używanie rozmiar jednostki przydziału 64 KB danych i plików dziennika, a także tymczasowe.

- **Najważniejsze wskazówki dotyczące zarządzania dysku**: podczas usuwania dyskiem danych lub zmiana jej typu pamięci podręcznej, Zatrzymaj usługę programu SQL Server podczas zmiany. Zmiana ustawień pamięci podręcznej na dysku z systemem operacyjnym Azure przestaje maszyn wirtualnych, zmienia typ pamięci podręcznej i ponownym uruchomieniu maszyn wirtualnych. Po zmianie ustawień pamięci podręcznej dysku danych, nie zatrzymaniu maszyn wirtualnych, ale dysku danych jest odłączony od maszyn wirtualnych podczas zmiany, a następnie ponownie nałożona.

    >[AZURE.WARNING] Zatrzymywanie usługi SQL Server podczas tych czynności błąd może spowodować uszkodzenie bazy danych.

## <a name="io-guidance"></a>Wskazówki dotyczące wejścia/wyjścia

- Najlepsze wyniki z nośnikami Premium zostaną osiągnięte, gdy parallelize żądania i aplikacji. Magazyn Premium jest przeznaczony dla scenariusze, w których Jo głębokość kolejki jest większa niż 1, tak zostanie wyświetlony niewielki lub wzrost wydajności w pojedynczym wątku szeregowa żądania (nawet gdy są one miejsca do magazynowania intensywnie). Może to na przykład mieć wpływ wyniki testu pojedynczym wątku narzędzi służących do analizowania wydajności, takich jak SQLIO.

- Warto rozważyć użycie [kompresji strony bazy danych](https://msdn.microsoft.com/library/cc280449.aspx) jako pomaga zwiększyć wydajność intensywna obciążenia We/Wy. Jednak kompresji danych może zwiększyć użycie Procesora na serwerze bazy danych.

- Należy rozważyć włączenie Inicjowanie pliku błyskawiczne skrócić czas wymagany dla alokacji początkowej plików. Aby skorzystać z inicjowanie błyskawiczne pliku, udzielanie konta usługi SQL Server (MSSQLSERVER) z SE_MANAGE_VOLUME_NAME i dodać go do zasady zabezpieczeń **Wykonywać zadania konserwacyjne głośność** . Jeśli korzystasz z obrazem platformy SQL Server dla Azure, domyślnego konta usługi (NT Service\MSSQLSERVER) nie zostanie dodane do zasad zabezpieczeń **Wykonywać zadania konserwacyjne głośność** . Innymi słowy inicjowanie błyskawiczne pliku nie jest włączona w obrazie platformy SQL Server Azure. Po dodaniu konta usługi programu SQL Server do zasad zabezpieczeń **Wykonywać zadania konserwacyjne głośność** , należy ponownie uruchomić usługę programu SQL Server. Może być zagadnienia dotyczące zabezpieczeń dla tej funkcji. Aby uzyskać więcej informacji zobacz [Zainicjowanie pliku bazy danych](https://msdn.microsoft.com/library/ms175935.aspx).

- **pomieścić** jest uważany za jedynie awaryjny nieoczekiwane wzrostu. Nie Zarządzaj do danych i dziennika wzrostu na codzienne z pomieścić. Użycie pomieścić wstępnie Powiększ za pomocą przełącznika rozmiar pliku.

- Upewnij się, że **autoshrink** jest wyłączone, aby uniknąć niepotrzebnego obciążenie, które mogą negatywnie wpłynąć na wydajność.

- Przenoszenie wszystkich baz danych na dyskach danych, w tym z baz danych systemu. Aby uzyskać więcej informacji zobacz [Przenoszenie systemowych baz danych](https://msdn.microsoft.com/library/ms345408.aspx).

- Przenoszenie programu SQL Server błędu dziennika i śledzenia pliku katalogów na dyskach danych. Można to zrobić w Menedżer konfiguracji programu SQL Server, klikając wystąpienie programu SQL Server i wybierając polecenie Właściwości. Na karcie **Parametry uruchamiania** można zmienić ustawienia pliku dziennika i śledzenia błędów. Katalog zrzutu określono na karcie **Zaawansowane** . Następujące zrzucie ekranu pokazano gdzie szukać parametr uruchamiania dziennika błędów.

    ![Zrzut ekranu dzienniku błędów SQL](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Ustawienia domyślnej lokalizacji pliku kopii zapasowej i bazy danych. W tym temacie za pomocą zalecenia i wprowadź odpowiednie zmiany w oknie dialogowym właściwości serwera. Aby uzyskać instrukcje zobacz [Przeglądanie lub zmienianie domyślnej lokalizacji danych i plików dziennika (program SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Następujące zrzut ekranu pokazuje, gdzie można wprowadzić te zmiany.

    ![Pliki dziennika programu SQL danych i tworzenia kopii zapasowych](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Włączanie zablokowanych stron w celu zmniejszenia Jo oraz wszelkie działania nawigacji między stronami. Aby uzyskać więcej informacji zobacz [Włączanie blokowania stron w pamięci opcja (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Jeśli korzystasz z programu SQL Server 2012, zainstaluj usługi dodatkiem Service Pack 1 Zbiorcza aktualizacja 10. Ta aktualizacja zawiera rozwiązanie problemu słabej wydajności na We/Wy podczas wykonywania wybierz do instrukcji tabeli tymczasowej w programu SQL Server 2012. Aby uzyskać informacje Zobacz ten [artykuł z bazy wiedzy knowledge base](http://support.microsoft.com/kb/2958012).

- Należy rozważyć, kompresując pliki danych podczas przenoszenia wewnątrz i na zewnątrz Azure.

## <a name="feature-specific-guidance"></a>Wskazówki dotyczące określonych funkcji

Niektórych wdrożeń może uzyskać dodatkowe korzyści przy użyciu bardziej zaawansowanych technik konfiguracji. Poniższa lista wyróżnienie niektóre funkcje programu SQL Server, które mogą ułatwić zwiększenie wydajności:

- **Wykonywanie kopii zapasowych do magazynu Azure**: podczas wykonywania kopii zapasowych dla SQL Server uruchomionym w środowisku maszyn wirtualnych Azure, można użyć [Narzędzia Kopia zapasowa programu SQL Server do adresu URL](https://msdn.microsoft.com/library/dn435916.aspx). Ta funkcja jest dostępna, począwszy od programu SQL Server 2012 z dodatkiem SP1 CU2 i zalecane dla wykonywanie kopii zapasowej na dyskach załączonych danych. Gdy zostanie kopia zapasowa i przywracanie z miejscem do magazynowania Azure, postępować zgodnie z zaleceniami w [Kopii zapasowej programu SQL Server do adresu URL najlepszymi rozwiązaniami i rozwiązywanie problemów i przywracanie z kopii zapasowych przechowywane w magazynie Azure](https://msdn.microsoft.com/library/jj919149.aspx). Można także zautomatyzować te kopie zapasowe funkcji [Automatycznego tworzenia kopii zapasowej dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-classic-sql-automated-backup.md).

    Przed programu SQL Server 2012 możesz użyć [Narzędzia Kopia zapasowa SQL Server Azure narzędzia](https://www.microsoft.com/download/details.aspx?id=40740). To narzędzie może pomóc zwiększyć przepustowość kopii zapasowej przy użyciu wielu kopii zapasowej pasek elementów docelowych.

- **Pliki danych programu SQL Server w Azure**: tej nowej funkcji, [Pliki danych programu SQL Server w Azure](https://msdn.microsoft.com/library/dn385720.aspx), jest dostępna, począwszy od programu SQL Server w 2014 r. Uruchomiony program SQL Server z plikami danych platformy Azure zaprezentowano porównywalna parametrów jako przy użyciu dysków Azure danych.

## <a name="next-steps"></a>Następne kroki

Jeśli interesuje Cię Poznawanie programu SQL Server i miejsca do magazynowania Premium bardziej szczegółowo, zobacz artykuł [Używanie Azure Premium miejsca do magazynowania z programem SQL Server w środowisku maszyn wirtualnych systemu](virtual-machines-windows-classic-sql-server-premium-storage.md).

Aby uzyskać najważniejsze wskazówki dotyczące zabezpieczeń zobacz [Zagadnienia dotyczące zabezpieczeń dla programu SQL Server w maszyn wirtualnych Azure](virtual-machines-windows-sql-security.md).

Przejrzyj inne tematy maszyn wirtualnych programu SQL Server w [Programu SQL Server na podglądzie maszyn wirtualnych Azure](virtual-machines-windows-sql-server-iaas-overview.md).
