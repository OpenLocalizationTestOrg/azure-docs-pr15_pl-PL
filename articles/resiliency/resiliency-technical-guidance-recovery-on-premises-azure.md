<properties
   pageTitle="Wytyczne techniczne: odzyskiwania ze źródeł lokalnych Azure | Microsoft Azure"
   description="Artykuł na opis i odzyskiwanie projektowania systemów z lokalnego infrastruktury Azure"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Wytyczne techniczne Azure elastyczność: odzyskiwania ze źródeł lokalnych Azure

Azure udostępnia pełnego zestawu usług umożliwiających rozszerzenia centrum danych lokalnych Azure celu wysokiej dostępności i awarii odzyskiwania:

* __Sieć__: Z wirtualną sieć prywatną bezpieczne rozszerzanie sieci lokalnej w chmurze.
* __Obliczanie__: Klienci korzystający z funkcji Hyper-V lokalnego można "Podnieś i shift" istniejące wirtualnych maszyn Azure.
* __Miejsca do magazynowania__: StorSimple rozszerza systemu plików do magazynu Azure. Usługa Azure kopii zapasowej przekazuje kopii zapasowych dotyczące plików i baz danych programu SQL Azure miejsca do magazynowania.
* __Replikacja bazy danych__: za pomocą programu SQL Server 2014 (lub nowszy) dostępność grup można zaimplementować wysoka dostępność i awarii odzyskiwanie danych lokalnych.

##<a name="networking"></a>Sieci

Wirtualna sieć Azure umożliwia tworzenie logicznie odizolowanych sekcji platformy Azure i bezpiecznie połączyć go z centrum danych w lokalnym lub komputerze klienckim pojedynczy przy użyciu połączenia protokołu IPsec. Z sieci wirtualnej możesz korzystać infrastruktury skalowalna, na żądanie w Azure zapewniając łączności z programem dane i aplikacje lokalnego, łącznie z systemem Windows Server, komputerów typu mainframe i UNIX. Aby uzyskać więcej informacji, zobacz [dokumentację networking Azure](../virtual-network/virtual-networks-overview.md) .

##<a name="compute"></a>Obliczenia

Jeśli używasz funkcji Hyper-V lokalnego, możesz "Podnieś i shift" istniejących maszyn wirtualnych Azure i usługodawców z systemem Windows Server 2012 (lub nowszy), bez wprowadzenie zmian w maszyn wirtualnych lub konwertowanie maszyn wirtualnych formaty. Aby uzyskać więcej informacji zobacz [temat dysków i wirtualnych dysków twardych dla Azure maszyn wirtualnych](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Odzyskiwanie witryny Azure

Jeśli chcesz awarii usługi (DRaaS), Azure udostępnia [Odzyskiwanie witryny Azure](https://azure.microsoft.com/services/site-recovery/). Odzyskiwanie witryny Azure zapewnia ochronę pełna VMware, funkcji Hyper-V i fizycznie serwera. Z Odzyskiwanie witryny Azure można użyć innego serwera lokalnego lub Azure jako witryny odzyskiwania. Aby uzyskać więcej informacji na odzyskiwanie witryny Azure zobacz [dokumentacji Odzyskiwanie witryny Azure](https://azure.microsoft.com/documentation/services/site-recovery/).

##<a name="storage"></a>Miejsca do magazynowania

Istnieje kilka opcji przy użyciu Azure kopii zapasowej witryn dla danych lokalnych.

###<a name="storsimple"></a>StorSimple

StorSimple bezpiecznie i przezroczysty integruje magazynu w chmurze dla aplikacji lokalnej. Zapewnia on również pojedynczego urządzenia, która zapewnia lokalny warstwowych wysokiej wydajności i magazynu w chmurze, archiwizowanie live ochrony danych w chmurze i odzyskiwanie. Aby uzyskać więcej informacji zobacz [StorSimple produktu strony](https://azure.microsoft.com/services/storsimple/).

###<a name="azure-backup"></a>Kopia zapasowa Azure

Azure kopia zapasowa umożliwia wykonywanie kopii zapasowych w chmurze za pomocą znanych narzędzi Kopia zapasowa systemu Windows Server 2012 (lub nowszy), Windows Server 2012 Essentials (lub nowszy) i System Centrum 2012 Data Protection Manager (lub nowszy). Te narzędzia umożliwiają przepływu pracy do zarządzania kopii zapasowej, która niezależnie od lokalizacji przechowywania kopii zapasowych, czy lokalnym dysku lub magazyn Azure. Po danych kopii zapasowej w chmurze, autoryzowani użytkownicy mogą łatwo można odzyskać kopie zapasowe do dowolnego serwera.

Z przyrostowe kopie zapasowe tylko zmian w plikach są przenoszone do chmury. Dzięki temu efektywne używanie ilości miejsca do magazynowania, zmniejszenie przepustowości i obsługi w chwili odzyskiwania wielu wersji danych. Możesz również użyć dodatkowe funkcje, takie jak zasady przechowywania danych, kompresja danych i ograniczania transfer danych. Za pomocą Azure lokalizację kopii zapasowej występują oczywiste korzyści kopie zapasowe są automatycznie "lokalizacjami". Dzięki temu dodatkowe wymagania dotyczące Zabezpieczanie i ochrona na miejscu nośnika kopii zapasowej.

Aby uzyskać więcej informacji, zobacz [Co to jest kopia zapasowa Azure?](../backup/backup-introduction-to-azure-backup.md) i [Konfigurowanie kopia zapasowa Azure DPM danych](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Bazy danych

Można mieć rozwiązania odzyskiwania systemu baz danych programu SQL Server w środowisku hybrydowym IT przy użyciu grupy dostępności (AlwaysOn), odzwierciedlające bazy danych, wysyłanie dziennika i kopii zapasowych i przywracania z magazynem obiektów Blob platformy Azure. Wszystkie te rozwiązania za pomocą programu SQL Server uruchomiony na maszyn wirtualnych Azure.

Grupy dostępności (AlwaysOn) można używać w środowisku hybrydowym IT gdzie repliki bazy danych istnieją zarówno w lokalnej i w chmurze. Przedstawiono na poniższym diagramie.

![Grupy dostępności (AlwaysOn) programu SQL Server we wdrożeniu hybrydowym w chmurze architektura](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Odzwierciedlające bazy danych mogą obejmować lokalne serwery i chmura w ustawieniach na podstawie certyfikatu. Na poniższym diagramie przedstawiono tej reguły.

![Odzwierciedlające bazy danych programu SQL Server w architekturze chmury hybrydowym](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Wysyłanie dziennika może służyć do synchronizacji bazy danych lokalnych z bazy danych programu SQL Server w Azure maszyn wirtualnych.

![Wysyłanie w architekturze chmury hybrydowych dziennika programu SQL Server](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Na koniec może wykonywać kopie zapasowe bazy danych lokalnych bezpośrednio z magazynem obiektów Blob platformy Azure.

![Tworzenie kopii zapasowych programu SQL Server z magazynem obiektów Blob platformy Azure w architekturze chmury hybrydowego](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

Aby uzyskać więcej informacji zobacz [wysokiej dostępności i awarii odzyskiwania dla programu SQL Server w środowisku maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) i [Kopia zapasowa i przywracanie dla programu SQL Server w środowisku maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Listy kontrolne odzyskiwania lokalnego w Microsoft Azure

###<a name="networking"></a>Sieci

  1. Zapoznaj się z sekcją sieci tego dokumentu.
  2. Aby zapewnić bezpieczne połączenie lokalnych z chmurą za pomocą wirtualnej sieci.

###<a name="compute"></a>Obliczenia

  1. Zapoznaj się z sekcją obliczeń tego dokumentu.
  2. Przenieś maszyny wirtualne między funkcji Hyper-V i Azure.

###<a name="storage"></a>Miejsca do magazynowania

  1. Zapoznaj się z sekcją miejsca do magazynowania w niniejszym dokumencie.
  2. Korzystać z usług StorSimple dotyczące korzystania z magazynu w chmurze.
  3. Za pomocą usługi Azure kopii zapasowej.

###<a name="database"></a>Bazy danych

  1. Zapoznaj się z sekcją bazy danych tego dokumentu.
  2. Warto rozważyć użycie programu SQL Server na maszyny wirtualne Azure jako kopii zapasowej.
  3. Konfigurowanie grupy dostępności (AlwaysOn).
  4. Konfigurowanie odzwierciedlające bazy danych na podstawie certyfikatu.
  5. Za pomocą wysyłanie dziennika.
  6. Wykonywanie kopii zapasowej bazy danych lokalnych z magazynem obiektów Blob platformy Azure.

##<a name="next-steps"></a>Następne kroki

Ten artykuł jest częścią serii ograniczony do [wytycznych technicznych elastyczność Azure](./resiliency-technical-guidance.md). Następny artykuł z tej serii jest [przypadkowego usunięcia lub uszkodzenia danych](./resiliency-technical-guidance-recovery-data-corruption.md).