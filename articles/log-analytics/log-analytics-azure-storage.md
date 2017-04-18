<properties
    pageTitle="Zbieranie danych Azure miejsca do magazynowania w omówienie analizy dziennika | Microsoft Azure"
    description="Zasoby Azure może zapisywać dzienników oraz metryki konto Azure miejsca do magazynowania, często przy użyciu zestawu narzędzi Diagnostyka Azure. Analizy dziennika można indeksu te dane w celu jej wyszukiwania."
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

# <a name="collecting-azure-storage-data-in-log-analytics-overview"></a>Zbieranie danych Azure miejsca do magazynowania w omówienie analizy dziennika

Wiele zasobów Azure będą mogli zapisać dzienniki i metryk konto Azure miejsca do magazynowania. Analizy dziennika można używać tych danych i ułatwić monitorowanie Azure zasobów.

Zapisywanie Azure miejsca do magazynowania zasób może Azure diagnostyki albo jeśli masz własny sposób zapisać danych. Te dane mogą być zapisane w różnych formatach do jednej z następujących lokalizacji:

+ Tabel platformy Azure
+ Obiektów blob platformy Azure
+ EventHub

Dziennik analizy obsługuje usług Azure modyfikujące danych przy użyciu [Azure dzienniki diagnostyczne](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md). Ponadto analizy dziennika obsługuje innych usług, które wyjściowy dzienniki i miar w różnych formatach i lokalizacji.  

>[AZURE.NOTE] Są naliczane szybkości normalny Azure danych do magazynowania i transakcje podczas wysyłania diagnostyki na koncie miejsca do magazynowania oraz podczas analizy dziennika odczytuje dane z Twojego konta miejsca do magazynowania.

![Diagram Azure magazynu](media/log-analytics-azure-storage/azure-storage-diagram.png)

## <a name="supported-azure-resources"></a>Obsługiwane platformy Azure zasobów

Analizy dziennika może zbierać dane dla Azure następujące zasoby:

| Typ zasobu | Dzienniki (kategorie diagnostyczne) | Rozwiązanie analizy dziennika |
| --------------------------------------- | -------------------------------- | --------------- |
| Wnioski aplikacji | Dostępność <br> Zdarzenia niestandardowe <br> Wyjątki <br> Żądania <br> | Wnioski aplikacji (wersja Preview) |
| Interfejs API zarządzania | | *Brak* (Wersja preview) |
| Automatyzacji <br> Microsoft.Automation/AutomationAccounts | JobLogs <br> JobStreams          | AzureAutomation (wersja Preview) |
| Kluczowe magazynu <br> Microsoft.KeyVault/Vaults               | AuditEvent                       | KeyVault (wersja Preview) |
| Brama aplikacji <br> Microsoft.Network/ApplicationGateways   | ApplicationGatewayAccessLog <br> ApplicationGatewayPerformanceLog | AzureNetworking (wersja Preview) |
| Grupa zabezpieczeń sieci <br> Microsoft.Network/NetworkSecurityGroups | NetworkSecurityGroupEvent <br> NetworkSecurityGroupRuleCounter | AzureNetworking (wersja Preview) |
| Usługa tkaninie                          | ETWEvent <br> Zdarzenia operacyjne <br> Niezawodne Aktor zdarzenia <br> Zdarzenia usługi zaufanego| ServiceFabric (wersja Preview) |
| Maszyn wirtualnych | Linux Syslog <br> Zdarzeń systemu Windows <br> Dziennik IIS <br> ETWEvent systemu Windows | *Brak* |
| Role w sieci Web <br> Role pracownika | Linux Syslog <br> Zdarzeń systemu Windows <br> Dziennik IIS <br> ETWEvent systemu Windows | *Brak* |

>[AZURE.NOTE] Monitorowania Azure maszyn wirtualnych (Linux i systemu Windows), zaleca się zainstalowanie [rozszerzenia maszyn wirtualnych analizy dziennika](log-analytics-azure-vm-extension.md). Agent dostarcza szczegółowego wniosków w środowisku maszyn wirtualnych systemu niż w przypadku korzystania diagnostyki zapisywane miejsca do magazynowania.

Pomaga nam Wyznaczanie priorytetów dodatkowe dzienników usługi OMS do analizowania przez głosowanie na naszych [Strona opinie](http://feedback.azure.com/forums/267889-azure-log-analytics/category/88086-log-management-and-log-collection-policy).


- Zobacz [Dzienniki diagnostyczne analizowanie Azure za pomocą analizy dziennika](log-analytics-azure-storage-json.md) , aby dowiedzieć się więcej o jak analizy dziennika można odczytywać dzienniki Azure usług, które obsługują [Azure dzienników diagnostycznych](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md):
  - Azure klucza magazynu
  - Azure automatyzacji
  - Brama aplikacji
  - Grupy zabezpieczeń sieci
- Zobacz [Magazyn obiektów blob Użyj programu IIS i tabeli miejsca do magazynowania dla wydarzeń](log-analytics-azure-storage-iis-table.md) Aby dowiedzieć się więcej na temat sposobu analizy dziennika można przeczytać dzienniki Azure usług tej diagnostyki zapisu magazyn tabel lub dzienniki programu IIS zapisywane blob miejscem do magazynowania, w tym:
  - Usługa tkaninie
  - Role w sieci Web
  - Role pracownika
  - Maszyn wirtualnych


Wnioski aplikacji znajduje się w prywatne Podgląd i jego zastosowania ciągły Eksportuj do blob miejsca do magazynowania. Aby dołączyć podgląd prywatne, skontaktuj się z zespołem Account Microsoft lub odwoływać się do szczegółowych informacji na temat [witryn opinii](https://feedback.azure.com/forums/267889-log-analytics/suggestions/6519248-integration-with-app-insights).

## <a name="next-steps"></a>Następne kroki

- [Dzienniki diagnostyczne analizowanie Azure za pomocą analizy dziennika](log-analytics-azure-storage-json.md) odczytywanie dzienniki Azure usług tej diagnostyki zapisu magazynem obiektów blob w formacie JSON.
- [Magazyn obiektów blob Użyj programu IIS i tabeli miejsca do magazynowania dla wydarzeń](log-analytics-azure-storage-iis-table.md) dzienniki dla Azure usług tej diagnostyki zapisu magazyn tabel lub dzienniki programu IIS zapisywane blob miejsca do magazynowania.
- [Włączanie rozwiązań](log-analytics-add-solutions.md) o podanie wgląd w danych.
- [Używanie kwerend wyszukiwania](log-analytics-log-searches.md) do analizy danych.
