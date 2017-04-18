<properties
    pageTitle="Co należy zrobić w przypadku, gdy awarii usługi Azure wpływa na Azure maszyn wirtualnych | Microsoft Azure"
    description="Dowiedz się, co należy zrobić w przypadku, gdy awarii usługi Azure wpływa na Azure maszyn wirtualnych."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Co należy zrobić w przypadku, gdy awarii usługi Azure wpływa na Azure maszyn wirtualnych

Firma Microsoft pracujemy nad trwałym upewnij się, że naszych usług zawsze są dla Ciebie dostępne, gdy ich potrzebujesz. Wymusza naszych niezależnych czasami wpływ nam w sposób powodujące zakłócenia w obsłudze niezaplanowane.

Firma Microsoft świadczy Umowa dotycząca poziomu usług (SLA) dla usług jako zobowiązanie łączności i czas pracy. Umowa dotycząca poziomu usług dla poszczególnych usług Azure można znaleźć w [Azure umowy](https://azure.microsoft.com/support/legal/sla/).

Azure zawiera już wiele funkcji wbudowanych platformy obsługujące aplikacje o wysokiej dostępności. Aby uzyskać więcej informacji o tych usług przeczytaj [Odzyskiwanie i wysoką dostępność dla aplikacji Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

W tym artykule opisano scenariuszu odzyskiwania PRAWDA awarii podczas całego regionu ulegnie awarii z powodu głównych naturalne awarii lub przerwa w świadczeniu usługi szerokie. Są to rzadkich wystąpienia, ale należy przygotować dla jest awarii całego regionu. Jeśli całego regionu napotka działaniu usługi, lokalnie zbędne kopie danych tymczasowo będzie niedostępne. Po włączeniu replikacji geo trzy dodatkowe kopie Azure magazyn obiektów blob i tabele są przechowywane w innym regionie. W przypadku awarii regionalne wykonane lub danych, w którym podstawowy region nie jest odzyskania Azure ponownie mapuje wszystkie wpisy DNS do regionu replikowane geo.

>[AZURE.NOTE]Należy pamiętać, że nie masz żadnej kontrolki przez ten proces i tylko wystąpi dla zakłócenia w obsłudze region na poziomie. Z tego powodu musi zależeć innych aplikacji strategii tworzenia kopii zapasowych uzyskanie najwyższego poziomu dostępności. Aby uzyskać więcej informacji zobacz sekcję strategii [danych awarii](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery).

Ułatwiające obsługę tych wystąpień rzadkich, firma Microsoft udostępnia następujące wskazówki dotyczące Azure maszyn wirtualnych w przypadku awarii usługi całego regionu, gdzie wdrożenie aplikacji Azure maszyn wirtualnych.

##<a name="option-1-wait-for-recovery"></a>Opcja 1: Poczekaj, aż odzyskiwania
W tym przypadku jest wymagane żadne działanie ze strony użytkownika. Pamiętaj, że pracujemy nad dokładnie przywrócić dostępność usługi. Można wyświetlić bieżący stan usługi w naszym [Pulpit nawigacyjny kondycji usługi Azure](https://azure.microsoft.com/status/).

>[AZURE.NOTE]To jest najlepszym rozwiązaniem, jeśli nie skonfigurowano Odzyskiwanie witryny Azure, wykonywanie kopii zapasowych maszyn wirtualnych, magazynowania zbędne geo dostęp do odczytu lub zbędne geo miejsca do magazynowania przed zakłóceń. Jeśli masz skonfigurowane zbędne geo miejsca do magazynowania lub miejsce do magazynowania zbędne geo dostęp do odczytu dla konta magazynu przechowywania maszyn wirtualnych wirtualnego dysku twardego (VHD), możesz Szukaj, aby odzyskać obrazu wirtualnego dysku twardego i próby obsługi administracyjnej nowych maszyn wirtualnych z niego. Nie jest preferowana opcja, ponieważ istnieje żadnych gwarancji synchronizacja danych, o ile kopii zapasowej maszyn wirtualnych Azure lub odzyskiwanie witryny Azure są używane. W związku z tym ta opcja nie jest gwarantowana do pracy.

Dla klientów, którzy mają mieć natychmiastowy dostęp do maszyn wirtualnych dostępne są następujące dwie opcje.  

>[AZURE.NOTE]Należy pamiętać, że jedną z następujących opcji mieć możliwość utratę danych.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>Opcja 2: Przywracanie maszyny z kopii zapasowej
W przypadku klientów, którzy skonfigurowali kopii zapasowej maszyn wirtualnych można przywrócić maszyn wirtualnych od punktu i przywracania kopii zapasowych.

Aby przywrócić nowych maszyn wirtualnych z kopii zapasowej Azure, zobacz [Przywracanie maszyn wirtualnych w Azure](../backup/backup-azure-restore-vms.md).

Aby ułatwić sobie planowanie infrastruktury kopii zapasowej Azure maszyn wirtualnych, zobacz [Planowanie infrastruktury kopii zapasowej maszyn wirtualnych w Azure](../backup/backup-azure-vms-introduction.md).

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Opcja 3: Inicjowanie trybie awaryjnym przy użyciu Odzyskiwanie witryny Azure
Jeśli masz skonfigurowane Azure Odzyskiwanie witryny do pracy z ryzyko Azure maszyn wirtualnych, można przywrócić pośrednictwem usługi SMS z ich repliki. Te repliki mogą znajdować się na Azure lub lokalnie. W takim przypadku możesz utworzyć nowy maszyn wirtualnych ze swoich istniejących replice. Aby przywrócić pośrednictwem usługi SMS z replice Odzyskiwanie witryny Azure, zobacz [Migrowanie IaaS Azure maszyn wirtualnych między regionami Azure z Odzyskiwanie witryny Azure](../site-recovery/site-recovery-migrate-azure-to-azure.md).

>[AZURE.NOTE]Mimo że Azure maszyn wirtualnych systemu operacyjnego i dyski danych będzie można replikować do pomocniczej wirtualnego dysku twardego, jeżeli są one zbędne geo miejsca do magazynowania lub konta magazynu zbędne geo dostęp do odczytu, każdego wirtualny dysk twardy jest replikowane niezależnie. Ten poziom replikacja nie daje gwarancji spójności w zreplikowanej wirtualnych dysków twardych. Jeśli aplikacji i/lub baz danych, które te dyski danych ma zależności od siebie, go nie jest gwarantowana że wszystkich wirtualnych dysków twardych są replikowane jako jeden migawkę. On również nie jest gwarantowana replice wirtualnego dysku twardego z zbędne geo miejsca do magazynowania lub odczytu zbędne geo magazynowania spowodują migawkę spójną aplikacji do uruchamiania maszyn wirtualnych.

##<a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat zaimplementowania strategii wysokiej dostępności i odzyskiwanie, zobacz [Odzyskiwanie i wysokiej dostępności aplikacji Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Opracowywaniu szczegółowy opis technicznych możliwości platformie chmurze, zobacz [wskazówki techniczne elastyczność Azure](../resiliency/resiliency-technical-guidance.md).

Aby dowiedzieć się, jak utworzyć kopię zapasową maszyny wirtualne, zobacz [Tworzenie kopii zapasowej Azure maszyn wirtualnych](../backup/backup-azure-vms.md).

Dowiedz się, jak dodać akompaniament i zautomatyzować ochrona usługi fizycznie (wirtualną) systemu Windows i Linux oraz urządzeń i które są uruchamiane na VMWare i maszyny wirtualne funkcji Hyper-V, zobacz [Odzyskiwanie witryny Azure](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)za pomocą Odzyskiwanie witryny Azure.

Jeśli instrukcje są wyczyść lub jeśli chcesz wykonywać operacje w Twoim imieniu przez firmę Microsoft, skontaktuj się z [Obsługą klienta](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
