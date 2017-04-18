<properties
    pageTitle="Co należy zrobić w przypadku Azure usługi zakłócenia w pracy, który ma wpływ usług w chmurze Azure | Microsoft Azure"
    description="Dowiedz się, co należy zrobić w przypadku awarii usługi Azure, który ma wpływ usług w chmurze Azure."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Co należy zrobić w przypadku Azure usługi zakłócenia w pracy, który ma wpływ usług w chmurze Azure

Firma Microsoft pracujemy nad trwałym upewnij się, że naszych usług zawsze są dla Ciebie dostępne, gdy ich potrzebujesz. Wymusza naszych niezależnych czasami wpływ nam w sposób powodujące zakłócenia w obsłudze niezaplanowane.

Firma Microsoft świadczy Umowa dotycząca poziomu usług (SLA) dla usług jako zobowiązanie łączności i czas pracy. Umowa dotycząca poziomu usług dla poszczególnych usług Azure można znaleźć w [Azure umowy](https://azure.microsoft.com/support/legal/sla/).

Azure zawiera już wiele funkcji wbudowanych platformy obsługujące aplikacje o wysokiej dostępności. Aby uzyskać więcej informacji o tych usług przeczytaj [Odzyskiwanie i wysoką dostępność dla aplikacji Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

W tym artykule opisano scenariuszu odzyskiwania PRAWDA awarii podczas całego regionu ulegnie awarii z powodu głównych naturalne awarii lub przerwa w świadczeniu usługi szerokie. Są to rzadkich wystąpienia, ale należy przygotować dla jest awarii całego regionu. Jeśli całego regionu napotka działaniu usługi, lokalnie zbędne kopie danych tymczasowo będzie niedostępne. Po włączeniu replikacji geo trzy dodatkowe kopie Azure magazyn obiektów blob i tabele są przechowywane w innym regionie. W przypadku awarii regionalne wykonane lub danych, w którym podstawowy region nie jest odzyskania Azure ponownie mapuje wszystkie wpisy DNS do regionu replikowane geo.

>[AZURE.NOTE]Należy pamiętać, że nie masz żadnej kontrolki przez ten proces i tylko wystąpi dla zakłócenia w centrum danych usługi. Z tego powodu musi zależeć innych aplikacji strategii tworzenia kopii zapasowych uzyskanie najwyższego poziomu dostępności. Aby uzyskać więcej informacji zobacz sekcję informacje o [danych strategie dotyczące awarii](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR). Jeśli chcesz można było wpływa na własny pracy awaryjnej, warto rozważ zastosowanie [odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage), co powoduje utworzenie kopii tylko do odczytu danych w innym regionie.

Ułatwiające obsługę tych wystąpień rzadkich, firma Microsoft udostępnia następujące wskazówki dotyczące Azure wirtualnych maszyn w przypadku awarii usługi całego regionu miejsce, w którym jest wdrożona aplikacja maszyn wirtualnych Azure.

##<a name="option-1-wait-for-recovery"></a>Opcja 1: Poczekaj, aż odzyskiwania
W tym przypadku jest wymagane żadne działanie ze strony użytkownika. Zapoznaj się dokładnie pracy Azure członkom zespołu do przywrócenia dostępność usługi. Można wyświetlić bieżący stan usługi w naszym [Pulpit nawigacyjny kondycji usługi Azure](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Jest najlepszym rozwiązaniem, jeśli klient nie skonfigurował Odzyskiwanie witryny Azure lub ma pomocniczą wdrożenia w innym regionie.

W przypadku klientów, którzy chcą mieć natychmiastowy dostęp do swoich usług w chmurze wdrożonym dostępne są następujące opcje.

>[AZURE.NOTE]Należy pamiętać, że te opcje mieć możliwość utratę danych.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Opcja 2: Wdrażanie ponownie konfiguracji usługi cloud do nowego regionu

Jeśli masz oryginalnego kodu, można po prostu tylko ponownego wdrożenia aplikacji, skojarzony konfiguracji i skojarzonych z nimi zasobów do nowej usługi w chmurze w regionie nowego.  

Aby uzyskać więcej szczegółów dotyczących sposobu tworzenia i wdrażania aplikacji usługi cloud zobacz [jak tworzenie i wdrażanie usługi w chmurze](./cloud-services-how-to-create-deploy-portal.md).

W zależności od źródła danych aplikacji może być konieczne sprawdzanie procedury odzyskiwania źródła danych aplikacji.
  * Magazyn Azure źródeł danych zobacz [Magazyn Azure replikacji](../storage/storage-redundancy.md#read-access-geo-redundant-storage) zaznacz opcje, które są dostępne w zależności od wybranego modelu replikacji dla aplikacji.
  * W przypadku źródeł bazy danych SQL, przeczytaj [Omówienie: firm ciągłości i bazy danych odzyskiwanie z bazą danych SQL w chmurze](../sql-database/sql-database-business-continuity.md) Aby sprawdzić opcje, które są dostępne w zależności od modelu replikacji wybranej aplikacji.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Opcja 3: Używanie kopii zapasowej wdrażania za pomocą Menedżera ruch Azure
Ta opcja założono, już zaprojektowaniem rozwiązania aplikacji z regionalne odzyskiwanie pamiętać. Za pomocą tej opcji, jeśli masz już wdrażanie aplikacji usług pomocniczej chmurze, którym działa w innym regionie i połączony za pośrednictwem kanału Menedżer ruchu. W tym przypadku sprawdzanie kondycji pomocniczej rozmieszczenia. Jeśli jest prawidłowy, można przekierowywać ruch do niego za pomocą Menedżera ruch Azure. Z tej strategii możesz korzystać metody routingu ruchu i konfiguracji kolejności pracy awaryjnej w Menedżerze ruch Azure. Aby uzyskać więcej informacji zobacz [Konfigurowanie ustawień Menedżer ruchu](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Równoważenia usług w chmurze Azure między różnymi regionami przy użyciu Menedżera ruch Azure](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat wdrożenia strategii wysokiej dostępności i odzyskiwanie, zobacz [Odzyskiwanie i wysokiej dostępności aplikacji Azure](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Opracowywaniu szczegółowy opis technicznych możliwości platformie chmurze, zobacz [wskazówki techniczne elastyczność Azure](../resiliency/resiliency-technical-guidance.md).

Jeśli instrukcje są wyczyść lub jeśli chcesz firmy Microsoft, aby wykonywać operacje w Twoim imieniu skontaktuj się z pomocą [Techniczną](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
