<properties
    pageTitle="Konfiguracja oceny rozwiązanie w dzienniku analizy | Microsoft Azure"
    description="Rozwiązanie ocena konfiguracji do analizy dziennika umożliwia szczegółowe informacje o bieżącym stanie infrastruktury serwera System Center Operations Manager przy użyciu programu Operations Manager agentów lub grupa zarządzania programu Operations Manager."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="configuration-assessment-solution-in-log-analytics"></a>Konfiguracja ocena rozwiązanie do analizy dziennika

Rozwiązanie ocena konfiguracji do analizy dziennika można łatwiej znaleźć potencjalne problemy dotyczące konfiguracji serwera za pomocą alertów i zalecenia wiedzy.

To rozwiązanie wymaga System Center Operations Manager. Ocena konfiguracji nie jest dostępna, jeśli używasz wyłącznie bezpośrednio połączone agentów.

Wyświetlanie pewne informacje w konfiguracji ocena rozwiązanie wymaga dodatek Silverlight dla przeglądarki.

>[AZURE.NOTE] Początek 5 lipca 2016 rozwiązanie ocena konfiguracji już nie można dodać do obszarów roboczych analizy dziennika i tego rozwiązania nie będzie już dostępne dla istniejących użytkowników po 1 sierpnia 2016. Dla klientów korzystających z tego rozwiązania dla programu SQL Server lub usługi Active Directory zaleca się, że możesz użyć [Programu SQL Server oceny](log-analytics-sql-assessment.md), [Active Directory oceny](log-analytics-ad-assessment.md) i [Active Directory stan replikacji](log-analytics-ad-replication-status.md) rozwiązania. Dla klientów korzystających z oceny konfiguracji dla systemu Windows funkcji Hyper-V, a Menedżer maszyn wirtualnych systemu Centrum, zaleca się korzystać z kolekcji zdarzeń i możliwości uzyskanie kompleksowy widok problemów w przypadku usługi środowisk śledzenia zmian.

![Konfiguracja ocena kafelków](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Konfiguracja dane są zbierane z serwerami monitorowane i następnie wysyłane do usługę w chmurze do przetwarzania. Logika jest stosowana do odbierane dane i usług w chmurze rekordów danych. Przetworzone dane dla serwerów są wyświetlane w następujących obszarach:

- **Alertów:** Pokazuje alerty związane z konfiguracją, aktywne, zgłaszane monitorowane serwerów. Te są wytwarzane przez reguły autorem Customer firmy Microsoft i pomocy technicznej firmy (CSS) z najlepszymi rozwiązaniami z pola.
- **Zalecenia wiedzy:** Pokazuje artykułów bazy wiedzy Microsoft Knowledge Base, które są zalecane dla obciążenia, które znajdują się w infrastrukturze; te są automatycznie sugerowane na podstawie swojej konfiguracji do używania nauki komputera.
- **Serwery i analizy obciążeń pracą:** Pokazuje serwerów i obciążenia, które są monitorowane usługi OMS.

![Pulpit nawigacyjny ocena konfiguracji](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Technologie można analizować oceny konfiguracji

Ocena konfiguracji usługi OMS analizuje obciążenia następujące czynności:

- Windows Server 2012 i Microsoft funkcji Hyper-V Server 2012
- Windows Server 2008 i Windows Server 2008 R2, w tym:
    - Usługi Active Directory
    - Host funkcji Hyper-V
    - Ogólne system operacyjny
- Program SQL Server 2008 lub nowszym
    - Aparat bazy danych programu SQL Server
- Program Microsoft SharePoint 2010
- Program Microsoft Exchange Server 2010 i Microsoft Exchange Server 2013
- Microsoft Lync Server 2013 i programu Lync Server 2010
- System Centrum 2012 z dodatkiem SP1 — Virtual Machine Manager

Dla programu SQL Server dla analizy są obsługiwane następujące wersji 32-bitowej i 64-bitowej:

- Program SQL Server 2016 — wszystkie wersje
- Program SQL Server 2014 - wszystkie wersje
- Program SQL Server 2008 i 2008 R2 — wszystkie wersje

Aparat bazy danych programu SQL Server analizy we wszystkich obsługiwanych wersjach. Ponadto 32-bitowej wersji programu SQL Server jest obsługiwana, gdy działa implementacji WOW64.

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie
Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji.

- Programu Operations Manager jest wymagana rozwiązanie ocena konfiguracji.
- Na każdym komputerze, na którym chcesz ocenić konfigurację, musisz mieć agenta programu Operations Manager.
- Dodaj rozwiązanie ocena konfiguracji do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  Nie jest wymagana żadna konfiguracja dalsze.

## <a name="configuration-assessment-data-collection-details"></a>Szczegóły zbioru danych ocena konfiguracji

Ocena konfiguracji zbiera dane konfiguracji, metadane i stan danych przy użyciu czynników, które są włączone.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane oceny konfiguracji.

| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Systemu Windows|![Brak](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Tak](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Brak](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![Tak](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Tak](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| dwa razy dziennie|

W poniższej tabeli pokazano przykłady typów danych zebranych przez ocena konfiguracji:

|**Typ danych**|**Pola**|
|---|---|
|Konfiguracja|Id_klienta, AgentID, identyfikator jednostki, ManagedTypeID, ManagedTypePropertyID, Bieżąca_wartość, ChangeDate|
|Metadane|BaseManagedEntityId, ObjectStatus, jednostka organizacyjna, ActiveDirectoryObjectSid, PhysicalProcessors, NazwaSieci, adres IP, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, adres IP, NetbiosDomainName, LogicalProcessors, Nazwa_dns, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Województwo|StateChangeEventId, StateId, NewHealthState, OldHealthState, kontekst, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, Ostatnia modyfikacja, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Alerty ocena konfiguracji
Można przeglądać i Zarządzanie alertami z oceny konfiguracji na stronie alerty. Alerty informujący o tym, ten problem, które zostało wykryte, przyczyny i jak rozwiązać ten problem. Zawierają także informacje o ustawienia konfiguracji w środowisku, w którym mogą powodować problemy z wydajnością.

![Wyświetlanie alertów](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] Alerty ocena konfiguracji różnią się od inne alerty były dzienniku analizy. Wyświetlanie alertów wymaga dodatek Silverlight dla przeglądarki.

Po zaznaczeniu elementu lub kategorii elementów na stronie alerty, pojawi się lista serwerów lub Obciążenie pracą z alertów, które dotyczą każdego elementu.

![Lista alertów](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Każdy alert zawiera łącze do artykułu z bazy wiedzy Microsoft Knowledge Base. Te artykuły zawierają dodatkowe informacje na temat alertu.

>[AZURE.TIP] Domyślnie maksymalna liczba alertów wyświetlana jest 2000. Aby wyświetlić więcej alertów, kliknij na pasku powiadomień powyżej listy alertów.

Kliknięcie dowolnego elementu na liście, aby wyświetlić artykuł z bazy wiedzy, która może pomóc w rozwiązaniu przyczyny problemu, który wygenerował alert.

![Artykuł z bazy wiedzy](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

Możesz zarządzać regułami alertów, aby zignorować określonych reguł lub rodzaje reguł.

![Zarządzaj regułami alertów](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Zalecenia dotyczące wiedzy
Podczas wyświetlania zalecenia wiedzy, są prezentowane wyniki wyszukiwania dziennika pozycje artykułach bazy wiedzy Microsoft KB zalecane dla obciążenia i komputerach, które zapewniają dodatkowe informacje na temat alertu.

![wyniki wyszukiwania, aby uzyskać zalecenia wiedzy](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Serwery i analizy obciążeń pracą
Podczas wyświetlania zalecenia wiedzy, aby wyświetlić wyniki wyszukiwania dziennika listy wszystkich serwerów i obciążenia, które nie są usługi OMS z programu Operations Manager.

![Serwery i obciążenia](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Następne kroki

- Umożliwia wyświetlanie danych oceny szczegółowe konfiguracji [wyszukiwania dziennika analizy dziennika](log-analytics-log-searches.md) .
