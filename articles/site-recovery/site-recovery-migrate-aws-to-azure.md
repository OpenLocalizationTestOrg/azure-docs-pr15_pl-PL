<properties
    pageTitle="Migrowanie maszyn wirtualnych systemu Windows z usługi sieci Web Amazon Azure z Odzyskiwanie witryny | Microsoft Azure"
    description="W tym artykule opisano, jak przeprowadzić migrację maszyn wirtualnych systemu Windows z systemem w usługach sieci Web Amazon (AWA) Azure za pomocą Odzyskiwanie witryny Azure."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/22/2016"
    ms.author="raynew"/>

#  <a name="migrate-windows-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migrowanie maszyn wirtualnych systemu Windows w usługach sieci Web Amazon (AWS) do Azure z Odzyskiwanie witryny Azure

## <a name="overview"></a>Omówienie

Witamy w usłudze Azure witryny odzyskiwania. Użyj tego artykułu, aby przeprowadzić migrację wystąpień systemu Windows w AWS Azure z Odzyskiwanie witryny. Przed rozpoczęciem należy zauważyć, że:

- Azure występują dwa modele rozmieszczania służące do tworzenia i pracy z zasobami: Menedżer zasobów Azure i klasyczny. Azure też ma dwa portali — portalu klasyczny Azure obsługuje model klasyczny wdrożenia i portal Azure z obsługą obu modeli wdrożenia. Podstawowe etapy migracji jest taka sama, czy w Menedżerze zasobów lub w klasycznym konfiguracji Odzyskiwanie witryny. Jednak instrukcjami interfejsu użytkownika i zrzutów ekranu w tym artykule dotyczą Azure portal.
- **Obecnie można tylko migrację ze AWS Azure. Może się nie powieść przez maszyny wirtualne z AWS Azure, ale możesz nie można powrócić je ponownie. Istnieje replikacja nie trwających.**
- Instrukcje dotyczące migracji w tym artykule są oparte na instrukcje dotyczące replikacji fizyczny komputer Azure. Zawiera łącza do kroków w [powielić maszyny wirtualne VMware lub serwerów fizycznych Azure](site-recovery-vmware-to-azure.md), które opisano, jak powielić fizyczny serwer w portalu Azure.
- Jeśli konfigurujesz Odzyskiwanie witryny w portalu klasycznym wykonaj szczegółowe instrukcje w [tym artykule](site-recovery-vmware-to-azure-classic.md). **Nie należy używać** z instrukcjami podanymi w tym [artykule starsze](site-recovery-vmware-to-azure-classic-legacy.md).

Publikowanie jakieś komentarze lub pytania u dołu tego artykułu lub [Azure odzyskiwania usług Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) w witrynie


## <a name="prerequisites"></a>Wymagania wstępne

Poniżej opisano, co jest potrzebne do tego wdrożenia

- **Serwer konfiguracji**: maszyn wirtualnych w lokalnym systemem Windows Server 2012 R2 działająca jako serwer konfiguracji. Zbyt zainstalować inne składniki Odzyskiwanie witryny (w tym na serwerze proces i wzorca docelowym serwerze) na tym maszyn wirtualnych. Przeczytaj więcej w [scenariuszu architektura](site-recovery-vmware-to-azure.md#scenario-architecture) i [wymagania wstępne dotyczące konfiguracji serwera](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **Wystąpienia maszyn wirtualnych EC2**: wystąpienia z systemem Windows, który chcesz przeprowadzić migrację.

## <a name="deployment-steps"></a>Kroków wdrażania

W tej sekcji opisano kroki wdrażania nowy portal Azure. Jeśli potrzebujesz tych kroków wdrażania dla Odzyskiwanie witryny w portalu klasyczny odwołują się do [tego artykułu](site-recovery-vmware-to-azure-classic.md).

1. [Tworzenie magazynu](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Wdrażanie serwera konfiguracji](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Po został wdrożony serwer konfiguracji, sprawdź, czy może komunikować się z maszyny wirtualne, które chcesz przeprowadzić migrację.
4. [Konfigurowanie ustawień replikacji](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Tworzenie zasad replikacji i przypisywanie ich do serwera konfiguracji.
5. [Instalowanie usługi mobilności](site-recovery-vmware-to-azure.md#step-6-replication-application). Każdy maszyn wirtualnych, które mają być chronione musi zainstalowana usługa mobilności. Ta usługa wysyła dane na serwer proces. Usługa mobilności można zainstalować ręcznie lub przypisany i instalowane automatycznie przez serwer proces po włączeniu ochrony dla maszyn wirtualnych. Reguły zapory na EC2 wystąpienia, które chcesz przeprowadzić migrację należy skonfigurowanym do obsługi instalacji wypychanych tej usługi. Grupa zabezpieczeń dla wystąpień EC2 powinny mieć następujące reguły:

    ![reguły zapory](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)

6. [Włączanie replikacji](site-recovery-vmware-to-azure.md#enable-replication). Włączanie replikacji maszyny wirtualne, którą chcesz zmigrować. Można wyszukiwać wystąpień EC2 przy użyciu prywatnych adresów IP, które można uzyskać z poziomu konsoli EC2.
7. [Uruchamianie nieplanowanego przełączania awaryjnego](site-recovery-failover.md#run-an-unplanned-failover). Po zakończeniu replikacji początkowej można uruchamiać nieplanowanego przełączania awaryjnego z AWS Azure dla każdego maszyn wirtualnych. Opcjonalnie można utworzyć plan odzyskiwania i uruchomić nieplanowanego przełączania awaryjnego, aby przeprowadzić migrację wiele maszyn wirtualnych z AWS Azure. [Aby uzyskać więcej informacji](site-recovery-create-recovery-plans.md) o planach odzyskiwania.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o innych sytuacjach replikacji w [Co to jest Odzyskiwanie witryny Azure?](site-recovery-overview.md)
