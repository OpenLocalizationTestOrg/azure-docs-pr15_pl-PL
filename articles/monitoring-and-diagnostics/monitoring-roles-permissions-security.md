<properties
    pageTitle="Wprowadzenie do ról, uprawnień i zabezpieczeń Monitor Azure | Microsoft Azure"
    description="Dowiedz się, jak za pomocą wbudowanych ról i uprawnień Azure Monitor ograniczenia dostępu do monitorowania zasobów."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="get-started-with-roles-permissions-and-security-with-azure-monitor"></a>Wprowadzenie do ról, uprawnień i zabezpieczeń Monitor Azure

Wiele członkom zespołu muszą ściśle regulowania dostęp do monitorowania danych i ustawień. Na przykład jeśli masz członków zespołu, którzy pracują wyłącznie na monitorowanie (inżynierów, inżynierów devops) lub użycie zarządzanych usługodawcę, być może zechcesz udzielić mu dostępu do tylko monitorowanie danych jednocześnie ograniczając możliwość tworzenia, modyfikowanie i usuwanie zasobów. W tym artykule pokazano, jak szybko zastosować wbudowany monitorowania roli RBAC użytkownikowi w Azure lub utworzyć własne niestandardowe roli dla użytkownika, który potrzebuje ograniczone uprawnienia monitorowania. Następnie go omówiono zagadnienia dotyczące bezpieczeństwa związanych z Azure Monitor zasobów i jak można ograniczyć dostęp do danych, które zawierają.

## <a name="built-in-monitoring-roles"></a>Wbudowane role monitorowania

Role wbudowanych Azure Monitor pozwalają ograniczyć dostęp do zasobów w subskrypcji podczas nadal włączania osób odpowiedzialnych za monitorowanie infrastruktury do konfigurowania tych danych. Monitorowanie Azure udostępnia dwie role w nowym polu: czytnik monitorowania i monitorowanie współautora.

### <a name="monitoring-reader"></a>Monitorowanie Reader

Osoby przypisana rola monitorowania czytnika może wyświetlić wszystkie dane monitorowania w subskrypcji, ale nie można modyfikować wszystkich zasobów lub edytować ustawienia dotyczące monitorowania zasobów. Ta rola jest odpowiedni dla użytkowników w organizacji, na przykład inżynierów pomocy technicznej lub operacji, które muszą mieć możliwość:

- Wyświetlanie monitorowania pulpitów nawigacyjnych w portalu i Utwórz własną prywatne monitorowania pulpity nawigacyjne.
- Kwerenda dotycząca metryki przy użyciu [Interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn931930.aspx), [poleceń cmdlet środowiska PowerShell](insights-powershell-samples.md)lub [interfejsu wiersza polecenia między platformami](insights-cli-samples.md).
- Kwerendy dziennik przy użyciu portalu, interfejsu API usługi REST Monitor Azure, poleceń cmdlet środowiska PowerShell lub interfejsu wiersza polecenia między platformami.
- Wyświetlanie [ustawień diagnostycznych](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) dla zasobu.
- Wyświetlanie [dziennika profilu](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) dla subskrypcji.
- Wyświetl ustawienia autoscale.
- Wyświetl aktywność alertów i ustawienia.
- Dostęp do danych wniosków aplikacji i wyświetlania w AI analizy danych.
- Wyszukiwanie danych obszaru roboczego usługi dziennika analizy (OMS) w tym danych dotyczących użycia dla obszaru roboczego.
- Wyświetlanie grup zarządzania usługi dziennika analizy (OMS).
- Pobieranie schematu wyszukiwania usługi dziennika analizy (OMS).
- Lista pakiety analizy usługi dziennika analizy (OMS).
- Pobieranie, a następnie wykonaj dziennika analizy (usługi OMS) zapisane wyszukiwania.
- Pobieranie konfiguracja magazynu usługi dziennika analizy (OMS).

> [AZURE.NOTE] Tej roli nie daje dostęp do odczytu danych dziennika, który strumieniowo do koncentratora zdarzenie lub przechowywane na koncie miejsca do magazynowania. [Zobacz poniżej](#security-considerations-for-monitoring-data) informacje dotyczące konfigurowania dostęp do tych zasobów.

### <a name="monitoring-contributor"></a>Monitorowanie trybu współautora

Osoby przypisana rola współautora monitorowania można wyświetlić wszystkie dane monitorowania w subskrypcji i tworzenie lub modyfikowanie ustawień monitorowania, ale nie można modyfikować inne zasoby. Ta rola jest podzbiorem roli czytnika monitorowania i jest odpowiedni dla członków zespołu monitorowania lub zarządzanych usługodawców, którzy Oprócz powyższych uprawnień także muszą mieć możliwość organizacji:

- Publikowanie pulpitów nawigacyjnych monitorowania jako udostępnionego pulpitu nawigacyjnego.
- Konfigurowanie [ustawień diagnostycznych](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) dla resource.*
- Ustawianie [profilu dziennika](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles) subscription.*
- Ustawianie alertów aktywności i ustawienia.
- Tworzenie wniosków aplikacji sieci web sprawdza i składniki.
- Listę klawiszy udostępniony obszar roboczy usługi dziennika analizy (OMS).
- Włączanie lub wyłączanie pakiety analizy usługi dziennika analizy (OMS).
- Tworzenie i usuwanie wykonać dziennika analizy (usługi OMS) zapisane wyszukiwania.
- Tworzenie i usuwanie konfiguracja magazynu usługi dziennika analizy (OMS).

* użytkownikowi niezależnie należy udzielić uprawnienie ListKeys zasobu docelowego (miejsca do magazynowania konta lub wydarzenia Centrum nazw) Aby ustawić profil dziennika lub ustawienie diagnostyczne.

> [AZURE.NOTE] Tej roli nie daje dostęp do odczytu danych dziennika, który strumieniowo do koncentratora zdarzenie lub przechowywane na koncie miejsca do magazynowania. [Zobacz poniżej](#security-considerations-for-monitoring-data) informacje dotyczące konfigurowania dostęp do tych zasobów.

## <a name="monitoring-permissions-and-custom-rbac-roles"></a>Monitorowanie uprawnienia i role RBAC niestandardowe
Jeśli powyższe role wbudowanych nie dokładnie potrzeb zespołu, możesz [utworzyć niestandardową rolę RBAC](../active-directory/role-based-access-control-custom-roles.md) uprawnieniami bardziej szczegółowego. Poniżej przedstawiono typowe operacje Azure Monitor RBAC z ich opisy.

| Operacja                                                   | Opis                                                                                                                                               |
|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Microsoft.Insights/AlertRules/[Read, zapis, usuwanie]         | Reguły alertów odczytu/zapisu/usuwanie.                                                                                                                            |
| Microsoft.Insights/AlertRules/Incidents/Read                | Lista zdarzeń (Historia alertu inicjowane) dla reguł alertów. Dotyczy to tylko do portalu.                                              |
| Microsoft.Insights/AutoscaleSettings/[Read, zapis, usuwanie]  | Ustawienia autoscale odczytu/zapisu/usuwanie.                                                                                                                     |
| Microsoft.Insights/DiagnosticSettings/[Read, zapis, usuwanie] | Ustawienia diagnostyczne odczytu/zapisu/usuwanie.                                                                                                                    |
| Microsoft.Insights/eventtypes/digestevents/Read             | To uprawnienie jest wymagane dla użytkowników, którzy muszą mieć dostęp do dzienników aktywności za pośrednictwem portalu.                                                                   |
| Microsoft.Insights/eventtypes/values/Read                   | Lista zdarzeń dziennika aktywności (zarządzania zdarzeniami) w subskrypcji. To uprawnienie dotyczy zarówno portalu, jak i programowy dostęp do dziennika aktywności. |
| Microsoft.Insights/LogDefinitions/Read                      | To uprawnienie jest wymagane dla użytkowników, którzy muszą mieć dostęp do dzienników aktywności za pośrednictwem portalu.                                                                   |
| Microsoft.Insights/MetricDefinitions/Read                   | W tym artykule metryczne definicje (lista dostępnych typów metryczne dla zasobu).                                                                                  |
| Microsoft.Insights/Metrics/Read                             | W tym artykule metryki dla zasobu.                                                                                                                              |

> [AZURE.NOTE] Dostęp do alertów, ustawień diagnostycznych i metryki dla zasobu wymaga, czy użytkownik ma dostęp do odczytu typ zasobu i zakres tego zasobu. Tworzenie ("Zapisz") diagnostyczne profil ustawienie lub dziennika, który archiwa do konta miejsca do magazynowania lub strumienie do zdarzenia koncentratorów wymaga użytkownikowi również uprawnień ListKeys do zasobu docelowego.

Na przykład za pomocą powyższej tabeli, można utworzyć niestandardową rolę RBAC "aktywności dziennika czytnika" tak jak poniżej:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Activity Log Reader"
$role.Description = "Can view activity logs."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Insights/eventtypes/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription")
New-AzureRmRoleDefinition -Role $role 
```

## <a name="security-considerations-for-monitoring-data"></a>Zagadnienia dotyczące zabezpieczeń dla monitorowania danych
Monitorowanie danych — szczególnie pliki dziennika — może zawierać informacje poufne, takie jak nazwy użytkowników lub adresy IP. Monitorowanie danych z platformy Azure oferuje trzy podstawowe formularzy programu:
1. Dziennik działań w tym artykule opisano wszystkie akcje kontrolnego kontrolki w subskrypcji usługi Azure.
2. Dzienniki diagnostyczne, które są dzienniki wysyłane przez zasób.
3. Wskaźniki, które są wysyłane przez wszystkie zasoby.

Wszystkie trzy typy danych można przechowywane na koncie miejsca do magazynowania lub strumieniowo do koncentratora zdarzenia, są one ogólnego przeznaczenia Azure zasobów. Ponieważ są one ogólnego przeznaczenia zasobów, tworzenie, usuwanie i uzyskiwanie do nich dostępu jest operacji uprzywilejowanych zwykle zastrzeżone przez administratora. Zaleca się używanie następujące wskazówki dotyczące zasobów związanych z monitorowania, aby zapobiec niewłaściwe użycie:

- Użyj konta jednego, dedykowane miejsca do magazynowania dla monitorowanie danych. Jeśli chcesz podzielić monitorowania na wiele kont miejsca do magazynowania, nigdy nie udostępniaj zastosowania konta miejsca do magazynowania między monitorowania i danych — monitorowanie, jak to przypadkowo mogą nadać potrzebują tylko dostęp do monitorowania danych (np. SIEM innych firm) dostęp do innych niż monitorowanie danych.
- Za pomocą jednego, dedykowane nazw Bus usługi lub Centrum zdarzeń przez wszystkie ustawienia diagnostyczne z tego samego powodu co powyżej.
- Ograniczyć dostęp do kont związanych z monitorowanie miejsca do magazynowania lub koncentratorów zdarzenia trzymając je w oddzielnej grupie zasobów, a [za pomocą zakresu](../active-directory/role-based-access-control-what-is.md#basics-of-access-management-in-azure) na poszczególnych ról monitorowania ograniczyć dostęp do tej grupy zasobów.
- Nigdy nie udzielanie uprawnień ListKeys dla kont miejsca do magazynowania lub koncentratorów zdarzenia w zakresie subskrypcji, gdy użytkownik tylko musi mieć dostęp do monitorowania danych. Zamiast tego przekazać te uprawnienia użytkownika na zasób lub grupa zasobów (Jeśli masz dedykowane monitorowania grupa zasobów) zakresu.

### <a name="limiting-access-to-monitoring-related-storage-accounts"></a>Ograniczanie dostępu do konta związane z monitorowanie miejsca do magazynowania
Gdy użytkownik lub aplikacja musi mieć dostęp do monitorowania danych na koncie miejsca do magazynowania, należy [wygenerować skojarzeń zabezpieczeń konta](https://msdn.microsoft.com/library/azure/mt584140.aspx) na koncie miejsca do magazynowania, zawierający dane z monitorowania z poziomu usług dostęp tylko do odczytu z magazynem obiektów blob. W programie PowerShell to może wyglądać na przykład:

```powershell
$context = New-AzureStorageContext -ConnectionString "[connection string for your monitoring Storage Account]"
$token = New-AzureStorageAccountSASToken -ResourceType Service -Service Blob -Permission "rl" -Context $context
```

Następnie można zapewnić tokenu do obiektu, że potrzebuje do odczytu z tego miejsca do magazynowania konta, a można wyświetlić listę i Odczyt wszystkich obiektów blob tego konta miejsca do magazynowania.

Alternatywnie Jeśli chcesz kontrolować to uprawnienie z RBAC można udzielić tego obiektu uprawnienie Microsoft.Storage/storageAccounts/listkeys/action dla tego konta określonego miejsca do magazynowania. Jest to konieczne dla użytkowników, którzy chcą mieć możliwość ustawienia diagnostyczne lub logowania profil, aby archiwizować konto miejsca do magazynowania. Można na przykład utworzyć następującą rolę RBAC niestandardowych dla użytkownika lub aplikacji, która musi tylko do odczytu z jednego miejsca do magazynowania konta:

```powershell
$role = Get-AzureRmRoleDefinition "Reader"
$role.Id = $null
$role.Name = "Monitoring Storage Account Reader"
$role.Description = "Can get the storage account keys for a monitoring storage account."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/storageAccounts/listkeys/action")
$role.Actions.Add("Microsoft.Storage/storageAccounts/Read")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/myMonitoringStorageAccount")
New-AzureRmRoleDefinition -Role $role 
```

> [AZURE.WARNING] Uprawnienie ListKeys umożliwia użytkownikowi listę klawiszy konta głównego i pomocniczego przestrzeni dyskowej. Klucze Udziel użytkownikowi wszystkie podpisane uprawnienia (Odczyt, zapis, tworzenie obiektów blob, Usuń obiektów blob itp.) we wszystkich podpisane services (obiektów blob, kolejki, tabeli, plik) tego konta miejsca do magazynowania. Zaleca się używanie skojarzeń zabezpieczeń konta opisany powyżej, jeśli to możliwe.

### <a name="limiting-access-to-monitoring-related-event-hubs"></a>Ograniczanie dostępu do koncentratorów zdarzeń związanych z monitorowania
Tego samego typu można stosować w przypadku koncentratorów zdarzenia, ale musisz najpierw utworzyć regułę autoryzacji dedykowane słuchać. Jeśli chcesz udzielić dostępu do aplikacji, która musi tylko słuchać koncentratory zdarzeń związanych z monitorowania, wykonaj następujące czynności:

1. Tworzenie zasad udostępniania na koncentratory zdarzenia, które zostały utworzone dla strumieniowego przesyłania danych monitorowania tylko słuchać roszczeń. To jest możliwe w portalu. Na przykład może połączenia go "monitoringReadOnly." Jeśli to możliwe można nadać tego klawisza bezpośrednio do konsumenta i przejść do następnego kroku.
2. Jeśli konsumenta musi mieć możliwość uzyskania klucza ad hoc, Udziel użytkownikowi akcji ListKeys koncentratora zdarzenia. To jest również dla użytkowników, którzy chcą mieć możliwość ustawienia diagnostyczne lub logowania profilu strumienia do koncentratorów zdarzenia. Na przykład można utworzyć regułę RBAC:

   ```powershell
   $role = Get-AzureRmRoleDefinition "Reader"
   $role.Id = $null
   $role.Name = "Monitoring Event Hub Listener"
   $role.Description = "Can get the key to listen to an event hub streaming monitoring data."
   $role.Actions.Clear()
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/authorizationrules/listkeys/action")
   $role.Actions.Add("Microsoft.ServiceBus/namespaces/Read")
   $role.AssignableScopes.Clear()
   $role.AssignableScopes.Add("/subscriptions/mySubscription/resourceGroups/myResourceGroup/providers/Microsoft.ServiceBus/namespaces/mySBNameSpace")
   New-AzureRmRoleDefinition -Role $role 
   ```


## <a name="next-steps"></a>Następne kroki
- [Przeczytaj o RBAC i uprawnienia w Menedżerze zasobów](../active-directory/role-based-access-control-what-is.md)
- [Zapoznanie się z omówieniem monitorowania platformy Azure](monitoring-overview.md)
