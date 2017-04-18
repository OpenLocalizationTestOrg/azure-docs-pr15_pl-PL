<properties
    pageTitle="RBAC: Role wbudowanych | Microsoft Azure"
    description="W tym temacie opisano gotowy w ról kontrola dostępu oparta na rolach (RBAC)."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/25/2016"
    ms.author="kgremban"/>

#<a name="rbac-built-in-roles"></a>RBAC: Role wbudowanych

Azure oparta na rolach programu Access kontrolki (RBAC) zawiera następujące role wbudowanych przypisanych do użytkowników, grupy i usług. Nie można zmodyfikować definicje ról wbudowane. Można jednak tworzyć [niestandardowe ról w Azure RBAC](role-based-access-control-custom-roles.md) do określonych potrzeb organizacji.

## <a name="roles-in-azure"></a>Role platformy Azure

Poniższa tabela zawiera krótkie opisy wbudowane role. Kliknij nazwę roli, aby wyświetlić szczegółowy wykaz **akcji** i **notactions** dla roli. Właściwości **akcji** Określa dozwolone akcje Azure zasobów. Ciągi akcji można używać symboli wieloznacznych. Właściwość **notactions** Określa akcje, które są wykluczone z dozwolone akcje.

>[AZURE.NOTE] Definicje ról Azure są stale rozwijających się. Ten artykuł jest aktualne w jak najszerszym zakresie, ale zawsze można znaleźć najnowsze definicje ról w programie PowerShell Azure. Użyć poleceń cmdlet `(get-azurermroledefinition "<role name>").actions` lub `(get-azurermroledefinition "<role name>").notactions` odpowiednio.

| Nazwa roli | Opis |
| --------- | ----------- |
| [Interfejs API zarządzania usługi współautorów](#api-management-service-contributor) | Można zarządzać usługi zarządzania interfejsu API |
| [Aplikacja wniosków składnik współautorów](#application-insights-component-contributor) | Można zarządzać składnikami wniosków aplikacji |
| [Operator automatyzacji](#automation-operator) | Można uruchomić, zatrzymać zawieszenia i wznawianie zadań |
| [BizTalk trybu współautora](#biztalk-contributor) | Mogą zarządzać usługami BizTalk |
| [Współautor ClearDB MySQL bazy danych](#cleardb-mysql-db-contributor) | Można zarządzać bazy danych ClearDB MySQL |
| [Trybu współautora](#contributor) | Można zarządzać wszystkich z wyjątkiem programu access. |
| [Współautorem Factory](#data-factory-contributor) | Tworzyć i zarządzać fabryki danych i zasoby podrzędny w nich. |
| [DevTest Labs użytkownika](#devtest-labs-user) | Można wyświetlić wszystkie elementy i połączyć, Rozpocznij, zamknięcia i ponownego uruchomienia maszyn wirtualnych |
| [Strefy DNS trybu współautora](#dns-zone-contributor) | Można zarządzać strefy DNS i rekordami |
| [Konto DocumentDB trybu współautora](#documentdb-account-contributor) | Można zarządzać kontami DocumentDB |
| [Systemy inteligentnego konta współautorów](#intelligent-systems-account-contributor) | Można zarządzać kontami systemach inteligentnych |
| [Sieć trybu współautora](#network-contributor) | Można zarządzać wszystkich zasobów sieciowych |
| [Nowy Relic APM konta trybu współautora](#new-relic-apm-account-contributor) | Można zarządzać kontami nowe zarządzania wydajnością aplikacji Relic i aplikacji |
| [Właściciel](#owner) | Można zarządzać wszystko, łącznie z programu access |
| [Czytnik](#reader) | Można wyświetlić wszystkie elementy, ale nie można wprowadzać zmian |
| [Redis pamięci podręcznej trybu współautora](#redis-cache-contributor) | Można zarządzać Redis pamięci podręcznej |
| [Harmonogram zadań zbiory współautorów](#scheduler-job-collections-contributor) | Czy zarządzanie zbiorami zadania harmonogramu |
| [Współautor usługi wyszukiwania](#search-service-contributor) | Można zarządzać usług wyszukiwania |
| [Menedżer zabezpieczeń](#security-manager) | Można zarządzać składniki zabezpieczeń, zasady zabezpieczeń i maszyn wirtualnych |
| [Współautor bazy danych SQL](#sql-db-contributor) | Można zarządzać baz danych programu SQL, ale nie ich zasady dotyczące zabezpieczeń |
| [Menedżer zabezpieczeń SQL](#sql-security-manager) | Można zarządzać zasady dotyczące zabezpieczeń programu SQL Server i baz danych |
| [Program SQL Server współautorów](#sql-server-contributor) | Można zarządzać serwerów SQL i baz danych, ale nie ich zasady dotyczące zabezpieczeń |
| [Klasyczny magazynowania konta współautorów](#classic-storage-account-contributor) | Można zarządzać kontami klasyczny miejsca do magazynowania |
| [Współautor konta miejsca do magazynowania](#storage-account-contributor) | Można zarządzać kontami miejsca do magazynowania |
| [Administrator dostępu użytkowników](#user-access-administrator) | Czy zarządzanie dostępem użytkowników do zasobów Azure |
| [Klasyczny maszyn wirtualnych trybu współautora](#classic-virtual-machine-contributor) | Można zarządzać klasyczny maszyn wirtualnych, ale nie wirtualnej sieci i magazynu konta z którym jest połączony |
| [Maszyn wirtualnych trybu współautora](#virtual-machine-contributor) | Można zarządzać w środowisku maszyn wirtualnych systemu, ale nie wirtualnej sieci i magazynu konta z którym jest połączony |
| [Klasyczny sieci trybu współautora](#classic-network-contributor) | Można zarządzać klasyczny wirtualnych sieci i zastrzeżone adresy IP |
| [Współautor Plan sieci Web](#web-plan-contributor) | Można zarządzać planów sieci web |
| [Współautor witryny sieci Web](#website-contributor) | Można zarządzać witryn sieci Web, ale nie w planach sieci web, z którymi są połączone |

## <a name="role-permissions"></a>Uprawnienia roli
W poniższych tabelach opisano określonych uprawnień do poszczególnych ról. Może to być **Akcje**, które udzielić uprawnień i **NotActions**, które ograniczyć je.

### <a name="api-management-service-contributor"></a>Interfejs API zarządzania usługi współautorów
Można zarządzać usługi zarządzania interfejsu API

| **Akcje** | |
| ------- | ------ |
| Microsoft.ApiManagement/Service/* | Tworzenie i zarządzanie nimi interfejsu API usługi zarządzania |
| Microsoft.Authorization/*/read | Przeczytaj autoryzacji |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Przeczytaj ról i przypisania ról |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="application-insights-component-contributor"></a>Aplikacja wniosków składnik współautorów
Można zarządzać składnikami wniosków aplikacji

| **Akcje** | |
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i przypisania ról |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.Insights/components/* | Tworzenie i zarządzanie nimi składniki wniosków |
| Microsoft.Insights/webtests/* | Tworzenie i zarządzanie nimi testy sieci web |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj kondycji zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="automation-operator"></a>Operator automatyzacji
Można uruchomić, zatrzymać zawieszenia i wznawianie zadań

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i przypisania ról |
| Microsoft.Automation/automationAccounts/jobs/read | Przeczytaj automatyzację zadań konta |
| Microsoft.Automation/automationAccounts/jobs/resume/action | Wznowić zadanie konta automatyzacji |
| Microsoft.Automation/automationAccounts/jobs/stop/action | Zatrzymywanie zadania konta automatyzacji |
| Microsoft.Automation/automationAccounts/jobs/streams/read | Przeczytaj automatyzacji strumienie zadania konta |
| Microsoft.Automation/automationAccounts/jobs/suspend/action | Zawieszenia zadania konta automatyzacji |
| Microsoft.Automation/automationAccounts/jobs/write | Pisanie automatyzację zadań konta |
| Microsoft.Automation/automationAccounts/jobSchedules/read | Przeczytaj harmonogram zadań konta automatyzacji |
| Microsoft.Automation/automationAccounts/jobSchedules/write | Przeczytaj harmonogram zadań konta automatyzacji |
| Microsoft.Automation/automationAccounts/read | Odczytywanie kont automatyzacji |
| Microsoft.Automation/automationAccounts/runbooks/read | Przeczytaj runbooks automatyzacji |
| Microsoft.Automation/automationAccounts/schedules/read | Przegląd automatyzacji arkuszy kont |
| Microsoft.Automation/automationAccounts/schedules/write | Pisanie automatyzacji arkuszy kont |
| Microsoft.Insights/components/* | Tworzenie i zarządzanie nimi składniki wniosków |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="biztalk-contributor"></a>BizTalk trybu współautora
Mogą zarządzać usługami BizTalk

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i przypisania ról |
| Microsoft.BizTalkServices/BizTalk/* | Tworzenie i zarządzanie usługami BizTalk |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="cleardb-mysql-db-contributor"></a>Współautor ClearDB MySQL bazy danych
Można zarządzać bazy danych ClearDB MySQL

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i przypisania ról |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |
| successbricks.cleardb/Databases/* | Tworzenie i zarządzanie bazami danych ClearDB MySQL |

### <a name="contributor"></a>Trybu współautora
Można zarządzać wszystkich z wyjątkiem programu access

| **Akcje** ||
| ------- | ------ |
| * | Tworzenie i zarządzanie zasobami wszystkie typy |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Authorization/*/Delete | Nie można usunąć role i przypisania ról |  
| Microsoft.Authorization/*/Write | Nie można utworzyć role i przypisania ról |

### <a name="data-factory-contributor"></a>Współautorem Factory
Tworzenie i zarządzanie fabryki danych i zasoby podrzędny w nich.

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.DataFactory/dataFactories/* | Tworzenie i zarządzanie fabryki danych i zasoby podrzędny w nich. |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="devtest-labs-user"></a>DevTest Labs użytkownika
Można wyświetlić wszystkie elementy i połączyć, Rozpocznij, zamknięcia i ponownego uruchomienia maszyn wirtualnych

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.Compute/availabilitySets/read | Właściwości zestawów dostępność do odczytu |
| Microsoft.Compute/virtualMachines/*/read | Więcej właściwości maszyny wirtualnej (maszyn wirtualnych rozmiarów stan czasu wykonywania, rozszerzenia maszyn wirtualnych, itp.) |
| Microsoft.Compute/virtualMachines/deallocate/action | Cofnąć maszyn wirtualnych |
| Microsoft.Compute/virtualMachines/read | Więcej właściwości maszyny wirtualnej |
| Microsoft.Compute/virtualMachines/restart/action | Ponowne uruchamianie maszyn wirtualnych |
| Microsoft.Compute/virtualMachines/start/action | Rozpoczynanie maszyn wirtualnych |
| Microsoft.DevTestLab/*/read | Więcej właściwości ćwiczenia |
| Microsoft.DevTestLab/labs/createEnvironment/action | Tworzenie środowiska ćwiczenia |
| Microsoft.DevTestLab/labs/formulas/delete | Usuwanie formuły |
| Microsoft.DevTestLab/labs/formulas/read | Przeczytaj formuł |
| Microsoft.DevTestLab/labs/formulas/write | Dodawanie lub modyfikowanie formuły |
| Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action | Ocenianie zasad ćwiczenia |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Dołączanie do puli adresu wewnętrznej bazy danych usługi równoważenia obciążenia |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Dołączanie do równoważenia obciążenia ruchu przychodzącego reguły translatora adresów Sieciowych |
| Microsoft.Network/networkInterfaces/*/read | Więcej właściwości karty sieciowej (na przykład wszystkie urządzenia do równoważenia obciążenia które interfejsu sieci jest częścią) |
| Microsoft.Network/networkInterfaces/join/action | Dołączanie do maszyny wirtualnej do karty sieciowej |
| Microsoft.Network/networkInterfaces/read | Przeczytaj interfejsów sieciowych |
| Microsoft.Network/networkInterfaces/write | Pisanie interfejsów sieciowych |
| Microsoft.Network/publicIPAddresses/*/read | Więcej właściwości publiczny adres IP |
| Microsoft.Network/publicIPAddresses/join/action | Dołączanie do publiczny adres IP |
| Microsoft.Network/publicIPAddresses/read | Przeczytaj sieci publicznych adresów IP |
| Microsoft.Network/virtualNetworks/subnets/join/action | Dołączanie do wirtualnej sieci |
| Microsoft.Resources/deployments/operations/read | Przeczytaj operacji rozmieszczania |
| Microsoft.Resources/deployments/read | Przeczytaj wdrożenia |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Storage/storageAccounts/listKeys/action | Lista miejsca do magazynowania konta klawiszy |

### <a name="dns-zone-contributor"></a>Strefy DNS trybu współautora
Można zarządzać strefy DNS i rekordy.

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/\*/Odczyt | Przeczytaj ról i przypisania ról |
| Microsoft.Insights/alertRules/\* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.Network/dnsZones/\* | Tworzenie i zarządzanie nimi strefy DNS i rekordy |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj kondycji zasobów |
| Microsoft.Resources/deployments/\* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/\* | Tworzenie i zarządzanie biletami pomocy technicznej |


### <a name="documentdb-account-contributor"></a>Konto DocumentDB trybu współautora
Można zarządzać kontami DocumentDB

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.DocumentDb/databaseAccounts/* | Tworzenie i zarządzanie kontami DocumentDB |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="intelligent-systems-account-contributor"></a>Systemy inteligentnego konta współautorów
Można zarządzać kontami systemach inteligentnych

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.IntelligentSystems/accounts/* | Tworzenie i zarządzanie kontami systemach inteligentnych |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="network-contributor"></a>Sieć trybu współautora
Można zarządzać wszystkich zasobów sieciowych

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.Network/* | Tworzenie i zarządzanie sieciami |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="new-relic-apm-account-contributor"></a>Nowy Relic APM konta trybu współautora
Można zarządzać kontami nowe zarządzania wydajnością aplikacji Relic i aplikacji

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |
| NewRelic.APM/accounts/* | Tworzenie i zarządzanie Relic nowej aplikacji wydajności zarządzania kontami |

### <a name="owner"></a>Właściciel
Można zarządzać wszystko, łącznie z programu access

| **Akcje** ||
| ------- | ------ |
| * | Tworzenie i zarządzanie zasobami wszystkie typy |

### <a name="reader"></a>Czytnik
Można wyświetlić wszystkie elementy, ale nie można wprowadzać zmian

| **Akcje** ||
| ------- | ------ |
| * / Odczyt | W tym artykule zasobów wszystkie typy, z wyjątkiem hasła. |

### <a name="redis-cache-contributor"></a>Redis pamięci podręcznej trybu współautora
Można zarządzać Redis pamięci podręcznej

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.Cache/redis/* | Tworzenie i zarządzanie nimi Redis pamięci podręcznej |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="scheduler-job-collections-contributor"></a>Harmonogram zadań zbiory współautorów
Czy zarządzanie zbiorami zadania harmonogramu

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |  
| Microsoft.Scheduler/jobcollections/* | Tworzenie i zarządzanie zbiorami zadania |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej  |

### <a name="search-service-contributor"></a>Współautor usługi wyszukiwania
Można zarządzać usług wyszukiwania

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |  
| Microsoft.Search/searchServices/* | Tworzenie i zarządzanie nimi usług wyszukiwania |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej  |

### <a name="security-manager"></a>Menedżer zabezpieczeń
Można zarządzać składniki zabezpieczeń, zasady zabezpieczeń i maszyn wirtualnych

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.ClassicCompute/*/read | Więcej informacji o konfiguracji klasyczny obliczyć maszyn wirtualnych |
| Microsoft.ClassicCompute/virtualMachines/*/write | Zapisać konfiguracji dla maszyn wirtualnych |
| Microsoft.ClassicNetwork/*/read | Przeczytaj informacje o sieci klasyczny konfiguracji  |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |  
| Microsoft.Security/* | Tworzenie i zarządzanie nimi i zasady zabezpieczeń składników |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej  |

### <a name="sql-db-contributor"></a>Współautor bazy danych SQL
Można zarządzać baz danych programu SQL, ale nie ich zasady dotyczące zabezpieczeń

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj ról i roli przydziałów |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Sql/servers/databases/* | Tworzenie i zarządzanie bazami danych SQL |
| Microsoft.Sql/servers/read | Przeczytaj serwerów SQL |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Nie można edytować zasady inspekcji |
| Microsoft.Sql/servers/databases/auditingSettings/* | Nie można edytować ustawień inspekcji |
| Microsoft.Sql/servers/databases/auditRecords/read  | Nie można odczytać rekordów inspekcji |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Nie można edytować zasady połączeń |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Nie można edytować danych maskowanie zasad |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Nie można edytować zasady alert zabezpieczeń |
| Microsoft.Sql/servers/databases/securityMetrics/* | Nie można edytować metryki zabezpieczeń |

### <a name="sql-security-manager"></a>Menedżer zabezpieczeń SQL
Można zarządzać zasady dotyczące zabezpieczeń programu SQL Server i baz danych

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Autoryzacja Microsoft odczytu |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów wniosków |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Sql/servers/auditingPolicies/* | Tworzenie i zarządzanie zasadami inspekcji programu SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Tworzenie i zarządzanie nimi ustawienia inspekcji serwera SQL |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Tworzenie i zarządzanie nimi zasad inspekcji bazy danych programu SQL server |
| Microsoft.Sql/servers/databases/auditingSettings/* | Tworzenie i zarządzanie nimi ustawienia inspekcji bazy danych programu SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read | Odczytywanie rekordów inspekcji |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Tworzenie i zarządzanie zasadami połączeń bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Tworzenie i zarządzanie nimi danych bazy danych programu SQL server maskowanie zasad |
| Microsoft.Sql/servers/databases/read | Bazy danych SQL odczytu |
| Microsoft.Sql/servers/databases/schemas/read | Schematy bazy danych serwera SQL odczytu |
| Microsoft.Sql/servers/databases/schemas/tables/columns/read | Kolumny tabeli bazy danych serwera SQL odczytu |
| Microsoft.Sql/servers/databases/schemas/tables/read | Tabele bazy danych serwera SQL odczytu |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Tworzenie i zarządzanie zasadami alertów zabezpieczeń bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/securityMetrics/* | Tworzenie i zarządzanie nimi metryki zabezpieczeń bazy danych serwera SQL |
| Microsoft.Sql/servers/read | Przeczytaj serwerów SQL |
| Microsoft.Sql/servers/securityAlertPolicies/* | Tworzenie i zarządzanie zasadami alertów zabezpieczeń programu SQL server |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="sql-server-contributor"></a>Program SQL Server współautorów
Można zarządzać serwerów SQL i baz danych, ale nie ich zasady dotyczące zabezpieczeń

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj autoryzacji|
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów wniosków |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Sql/servers/* | Tworzenie i zarządzanie nimi serwerów SQL |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

| **NotActions** ||
| ------- | ------ |
| Microsoft.Sql/servers/auditingPolicies/* | Nie można edytować zasady inspekcji programu SQL server |
| Microsoft.Sql/servers/auditingSettings/* | Nie można edytować ustawienia inspekcji programu SQL server |
| Microsoft.Sql/servers/databases/auditingPolicies/* | Nie można edytować zasady inspekcji bazy danych programu SQL server |
| Microsoft.Sql/servers/databases/auditingSettings/* | Nie można edytować ustawienia inspekcji bazy danych programu SQL server |
| Microsoft.Sql/servers/databases/auditRecords/read  | Nie można odczytać rekordów inspekcji |
| Microsoft.Sql/servers/databases/connectionPolicies/* | Nie można edytować zasady połączeń bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/dataMaskingPolicies/* | Nie można edytować danych bazy danych programu SQL server maskowanie zasad |
| Microsoft.Sql/servers/databases/securityAlertPolicies/* | Nie można edytować zasady alertów zabezpieczeń bazy danych serwera SQL |
| Microsoft.Sql/servers/databases/securityMetrics/* | Nie można edytować metryki zabezpieczeń bazy danych serwera SQL |
| Microsoft.Sql/servers/securityAlertPolicies/* | Nie można edytować zasady alertów zabezpieczeń serwera SQL |

### <a name="classic-storage-account-contributor"></a>Klasyczny magazynowania konta współautorów
Można zarządzać kontami klasyczny miejsca do magazynowania

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj autoryzacji |
| Microsoft.ClassicStorage/storageAccounts/* | Tworzenie i zarządzanie kontami miejsca do magazynowania |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów wniosków |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |  
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="storage-account-contributor"></a>Współautor konta miejsca do magazynowania
Można zarządzać kontami miejsca do magazynowania, ale nie możesz uzyskać do nich dostępu do.

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Czytanie wszystkich autoryzacji |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów wniosków |
| Microsoft.Insights/diagnosticSettings/* | Zarządzanie ustawieniami diagnostyczne |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |  
| Microsoft.Storage/storageAccounts/* | Tworzenie i zarządzanie kontami miejsca do magazynowania |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="user-access-administrator"></a>Administrator dostępu użytkowników
Czy zarządzanie dostępem użytkowników do zasobów Azure

| **Akcje** ||
| ------- | ------ |
| * / Odczyt | W tym artykule zasobów wszystkie typy, z wyjątkiem hasła. |
| Microsoft.Authorization/* | Zarządzanie autoryzacji |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="classic-virtual-machine-contributor"></a>Klasyczny maszyn wirtualnych trybu współautora
Można zarządzać klasyczny maszyn wirtualnych, ale nie wirtualnej sieci i magazynu konta z którym jest połączony

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj autoryzacji |
| Microsoft.ClassicCompute/domainNames/* | Tworzenie i zarządzanie nazwami domen klasyczny obliczeń |
| Microsoft.ClassicCompute/virtualMachines/* | Tworzenie i zarządzanie nimi maszyn wirtualnych |
| Microsoft.ClassicNetwork/networkSecurityGroups/join/action | Dołączanie do grupy zabezpieczeń sieci |
| Microsoft.ClassicNetwork/reservedIps/link/action | Łącze zastrzeżone adresy IP |
| Microsoft.ClassicNetwork/reservedIps/read | Adresy IP zastrzeżone odczytu |
| Microsoft.ClassicNetwork/virtualNetworks/join/action | Dołączanie do wirtualnego sieci |
| Microsoft.ClassicNetwork/virtualNetworks/read | Przeczytaj wirtualnych sieci |
| Microsoft.ClassicStorage/storageAccounts/disks/read | Więcej miejsca do magazynowania dysków konta |
| Microsoft.ClassicStorage/storageAccounts/images/read | Więcej miejsca do magazynowania konta obrazów |
| Microsoft.ClassicStorage/storageAccounts/listKeys/action | Lista miejsca do magazynowania konta klawiszy |
| Microsoft.ClassicStorage/storageAccounts/read | Odczytywanie kont klasyczny miejsca do magazynowania |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów wniosków |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="virtual-machine-contributor"></a>Maszyn wirtualnych trybu współautora
Można zarządzać w środowisku maszyn wirtualnych systemu, ale nie wirtualnej sieci i magazynu konta z którym jest połączony

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj autoryzacji |
| Microsoft.Compute/availabilitySets/* | Tworzenie zestawów i zarządzanie nimi obliczeń dostępności |
| Microsoft.Compute/locations/* | Tworzenie i zarządzanie lokalizacjami obliczeń |
| Microsoft.Compute/virtualMachines/* | Tworzenie i zarządzanie nimi maszyn wirtualnych |
| Microsoft.Compute/virtualMachineScaleSets/* | Tworzenie zestawów i zarządzanie nimi maszyn wirtualnych skali |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów wniosków |
| Microsoft.Network/applicationGateways/backendAddressPools/join/action | Dołączanie do adresu sieci pule bramy wewnętrznej bazy danych |
| Microsoft.Network/loadBalancers/backendAddressPools/join/action | Dołączanie do puli adresów wewnętrznej bazy danych usługi równoważenia obciążenia |
| Microsoft.Network/loadBalancers/inboundNatPools/join/action | Dołączanie do równoważenia obciążenia ruchu przychodzącego pul translatora adresów Sieciowych |
| Microsoft.Network/loadBalancers/inboundNatRules/join/action | Dołączanie do równoważenia obciążenia translatora adresów Sieciowych reguły przychodzące |
| Microsoft.Network/loadBalancers/read | Urządzenia do równoważenia obciążenia odczytu |
| Microsoft.Network/locations/* | Tworzenie i zarządzanie nimi lokalizacje sieciowe |
| Microsoft.Network/networkInterfaces/* | Tworzenie i zarządzanie nimi interfejsów sieciowych |
| Microsoft.Network/networkSecurityGroups/join/action | Dołączanie do grupy zabezpieczeń sieci |
| Microsoft.Network/networkSecurityGroups/read | Czytanie grup zabezpieczeń sieci |
| Microsoft.Network/publicIPAddresses/join/action | Dołączanie do sieci publicznych adresów IP |
| Microsoft.Network/publicIPAddresses/read | Przeczytaj sieci publicznych adresów IP |
| Microsoft.Network/virtualNetworks/read | Przeczytaj wirtualnych sieci |
| Microsoft.Network/virtualNetworks/subnets/join/action | Dołączanie do wirtualnego podsieci |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |  
| Microsoft.Storage/storageAccounts/listKeys/action | Lista miejsca do magazynowania konta klawiszy |
| Microsoft.Storage/storageAccounts/read | Odczytywanie kont miejsca do magazynowania |
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="classic-network-contributor"></a>Klasyczny sieci trybu współautora
Można zarządzać klasyczny wirtualnych sieci i zastrzeżone adresy IP

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj autoryzacji |
| Microsoft.ClassicNetwork/* | Tworzenie i zarządzanie sieciami klasyczny |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów wniosków |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |  
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |

### <a name="web-plan-contributor"></a>Współautor Plan sieci Web
Można zarządzać planów sieci web

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj autoryzacji |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów wniosków |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |  
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |
| Microsoft.Web/serverFarms/* | Tworzenie i zarządzanie nimi farmy serwerów |

### <a name="website-contributor"></a>Współautor witryny sieci Web
Można zarządzać witryn sieci Web, ale nie w planach sieci web, z którymi są połączone

| **Akcje** ||
| ------- | ------ |
| Microsoft.Authorization/*/read | Przeczytaj autoryzacji |
| Microsoft.Insights/alertRules/* | Tworzenie i zarządzanie regułami alertów wniosków |
| Microsoft.Insights/components/* | Tworzenie i zarządzanie nimi składniki wniosków |
| Microsoft.ResourceHealth/availabilityStatuses/read | Przeczytaj zdrowia zasobów |
| Microsoft.Resources/deployments/* | Tworzenie i zarządzanie nimi wdrożeniach grupa zasobów |
| Microsoft.Resources/subscriptions/resourceGroups/read | Czytanie grup zasobów |  
| Microsoft.Support/* | Tworzenie i zarządzanie biletami pomocy technicznej |
| Microsoft.Web/certificates/* | Tworzenie i zarządzanie nimi certyfikaty witryny sieci Web |
| Microsoft.Web/listSitesAssignedToHostName/read | Dostęp do witryn przypisane do nazwy hosta |
| Microsoft.Web/serverFarms/join/action | Dołączanie do farmy serwerów |
| Microsoft.Web/serverFarms/read | Przeczytaj farmy serwerów |
| Microsoft.Web/sites/* | Tworzenie i zarządzanie witrynami sieci Web |

## <a name="see-also"></a>Zobacz też
- [Kontrola dostępu oparta na rolach](role-based-access-control-configure.md): rozpoczynanie pracy z RBAC w portalu Azure.
- [Role niestandardowe w Azure RBAC](role-based-access-control-custom-roles.md): Dowiedz się, jak tworzyć niestandardowe role do potrzeb użytkownika programu access.
- [Tworzenie dostępu zmienić raport historii](role-based-access-control-access-change-history-report.md): śledzenie informacji dotyczących Zmienianie przypisania ról w RBAC.
- [Kontrola dostępu oparta na rolach rozwiązywania problemów](role-based-access-control-troubleshooting.md): pobieranie sugestii dotyczących rozwiązywania typowych problemów.
