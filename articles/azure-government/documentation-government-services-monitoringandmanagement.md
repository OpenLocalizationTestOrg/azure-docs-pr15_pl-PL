<properties
    pageTitle="Azure dokumentacji dla instytucji rządowych | Microsoft Azure"
    description="Umożliwia porównanie funkcji i wskazówki dotyczące tworzenia aplikacji dla instytucji rządowych Azure."
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-monitoring-and-management"></a>Dla instytucji rządowych Azure monitorowania i zarządzania

W tym artykule omówiono, monitorowania i zarządzania usług odmiany i uwagi dotyczące środowiska Azure dla instytucji rządowych.

## <a name="automation"></a>Automatyzacji

Automatyzacja jest zazwyczaj dostępny w Azure dla instytucji rządowych.

### <a name="variations"></a>Odmiany

Następujące funkcje automatyzacji nie są obecnie dostępne w Azure dla instytucji rządowych.

+ Tworzenie poświadczenia zasady usług uwierzytelniania

Aby uzyskać więcej informacji zobacz [dokumentację publicznej automatyzacji](../automation/automation-intro.md).

## <a name="log-analytics"></a>Analizy dziennika

Analizy dziennika jest zazwyczaj dostępny w Azure dla instytucji rządowych.

### <a name="variations"></a>Odmiany

Następujące funkcje analizy dziennika i rozwiązań nie są obecnie dostępne w Azure dla instytucji rządowych.

+ Rozwiązania, które są w podglądzie Microsoft Azure, w tym:
  - Rozwiązania monitorowania sieci
  - Rozwiązanie zależności monitorowanie aplikacji
  - Rozwiązanie usługi Office 365
  - Rozwiązanie systemu Windows 10 uaktualnienie analizy
  - Rozwiązanie wniosków aplikacji
  - Rozwiązanie Azure analizy sieci
  - Rozwiązanie Azure analizy automatyzacji
  - Rozwiązanie analizy magazynu klucza
+ Rozwiązania i funkcje, które wymagają aktualizacji oprogramowania lokalnego, w tym:
  - Integracja z systemu Centrum Operations Manager 2016 (wcześniejszych wersji programu Operations Manager są obsługiwane)
  - Grupy komputerów za pomocą Menedżera konfiguracji centrum systemu
  - Powierzchniowy rozwiązanie koncentratora
+ Funkcje, które są w podglądzie publicznej Azure, w tym:
  - Eksportowanie danych do usługi Power BI
+ Metryki Azure i diagnostyce Azure
+ Operacje zarządzania pakiet aplikacji dla urządzeń przenośnych

Adresy URL dla analizy dziennika różnią się dla instytucji rządowych Azure:

| Azure publicznej | Azure dla instytucji rządowych | Notatki |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Portal analizy dziennika |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Dane zbierających interfejsu API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent komunikacji — [Konfigurowanie ustawień zapory](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent komunikacji — [Konfigurowanie ustawień zapory](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent komunikacji — [Konfigurowanie ustawień zapory](../log-analytics/log-analytics-proxy-firewall.md) |


Następujące funkcje analizy dziennika działają inaczej Azure dla instytucji rządowych:

+ Agent systemu Windows musi zostać pobrany z [dziennika analizy portal](https://oms.microsoft.us) Azure administracji.
+ Aby połączyć serwera zarządzania System Center Operations Manager analizy dziennika, musisz pobrać i importowanie zaktualizowane pakiety zarządzania.
  1. Pobierz i Zapisz [zaktualizowany pakietów zarządzania](http://go.microsoft.com/fwlink/?LinkId=828749).
  2. Rozpakuj plik pobranego pliku.
  3. Pakiety zarządzania zaimportować do programu Operations Manager. Aby uzyskać informacje na temat importowania pakiet zarządzania z dysku, zobacz [jak zaimportować pakiet zarządzania menedżera operacji](http://technet.microsoft.com/library/hh212691.aspx) w witrynie Microsoft TechNet w sieci Web.
  4. Aby połączyć programu Operations Manager do analizy dziennika, wykonaj czynności opisane w [Łączenie analizy dziennika programu Operations Manager](../log-analytics/log-analytics-om-agents.md).


## <a name="frequently-asked-questions"></a>Często zadawane pytania

+ Można przeprowadzić migrację danych z dziennika analizy w programie Microsoft Azure dla instytucji rządowych Azure?
  - Wartość nie. Nie jest możliwe przenieść dane lub obszaru roboczego z Microsoft Azure Azure dla instytucji rządowych.
+ Można przełączać się między Microsoft Azure i dla instytucji rządowych Azure obszarów roboczych z portalu operacje zarządzania pakietu dziennika analizy?
  - Wartość nie. Portali Microsoft Azure i dla instytucji rządowych Azure są oddzielone i nie udostępniaj informacji.

Aby uzyskać więcej informacji dokumentacji [analizy dziennika publicznej](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Następne kroki

Informacje dodatkowe i aktualizacji subskrybowanie <a href="https://blogs.msdn.microsoft.com/azuregov/">Blog dotyczący programu Microsoft Azure dla instytucji rządowych.</a>
