<properties
    pageTitle="Azure dokumentacji dla instytucji rządowych | Microsoft Azure"
    description="Umożliwia porównanie funkcji i wskazówki dotyczące tworzenia aplikacji dla instytucji rządowych Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="scooxl"
    manager="zakramer"
    editor=""/>
<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/25/2016"
    ms.author="scooxl"/>
#  <a name="azure-government-management-and-security"></a>Zarządzanie Azure dla instytucji rządowych i zabezpieczeń

## <a name="automation"></a>Automatyzacji

Automatyzacja jest zazwyczaj dostępny w Azure dla instytucji rządowych.

### <a name="variations"></a>Odmiany

Następujące funkcje automatyzacji nie są obecnie dostępne w Azure dla instytucji rządowych.

+ Tworzenie poświadczenia zasady usług uwierzytelniania

Aby uzyskać więcej informacji zobacz [dokumentację publicznej automatyzacji](../automation/automation-intro.md).


##  <a name="key-vault"></a>Kluczowe magazynu
Aby uzyskać szczegółowe informacje na tej usługi i jak używać tej funkcji, zobacz <a href="https://azure.microsoft.com/documentation/services/key-vault">Azure klucza magazynu dokumentacji publicznej.</a>
### <a name="data-considerations"></a>Zagadnienia dotyczące danych
Poniższe informacje identyfikuje granicę dla instytucji rządowych Azure dla Azure klucza magazynu:

| Dane regulacji kontrolowane dozwolone | Dane regulacji kontrolowane niedozwolone |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Wszystkie dane są szyfrowane za pomocą klucza Azure klucza magazynu może zawierać dane Regulated kontrolowane. | Azure klucza magazynu metadanych nie jest dozwolona zawierać kontrolowane Eksportuj dane. Metadane to między innymi wszystkie dane konfiguracji wprowadzone podczas tworzenia i przechowywania do magazynu klucza.  Nie wprowadzaj dane Regulated kontrolowane w następujących polach: nazwy grupy zasobów i klucza magazynu, nazwy subskrypcji. |

Klucz magazynu jest zazwyczaj dostępny w Azure dla instytucji rządowych. Jak publicznej istnieje bez rozszerzenia, więc klucz magazynu jest dostępne za pośrednictwem programu PowerShell i polecenie tylko.
## <a name="log-analytics"></a>Analizy dziennika
Analizy dziennika jest zazwyczaj dostępny w Azure dla instytucji rządowych. 

### <a name="variations"></a>Odmiany

Następujące funkcje analizy dziennika i rozwiązań nie są obecnie dostępne w Azure dla instytucji rządowych. Ta lista jest aktualizowana po stan funkcji / zmienia rozwiązań.

+ Rozwiązania, które są w podglądzie publicznej Azure, w tym:
  - Rozwiązania sieci monitorowania
  - Monitorowania zależności aplikacji
  - Rozwiązanie usługi Office 365
  - Rozwiązanie systemu Windows 10 uaktualnienie analizy
  - Wnioski aplikacji
  - Rozwiązanie Azure analizy sieci
  - Automatyzacja Azure analizy
  - Analizy klucza magazynu
+ Rozwiązania i funkcje, które wymagają aktualizacji oprogramowania lokalnego, łącznie z
  - Integracja z systemu Centrum Operations Manager 2016 (wcześniejszych wersji programu Operations Manager są obsługiwane)
  - Grupy komputerów za pomocą Menedżera konfiguracji centrum systemu
  - Powierzchniowy rozwiązanie Centrum
+ Funkcje, które są w podglądzie publicznej Azure, łącznie z
  - Eksportowanie danych do PowerBI
+ Metryki Azure i diagnostyce Azure
+ Aplikacje usługi OMS Mobile

Adresy URL dla analizy dziennika różnią się dla instytucji rządowych Azure:

| Azure publicznej | Azure dla instytucji rządowych | Notatki |
|--------------|------------------|-------|
| MMS.microsoft.com | OMS.microsoft.us | Portal analizy dziennika |
| *workspaceId*. ods.opinsights.azure.com | *workspaceId*. ods.opinsights.azure.us | [Dane zbierających interfejsu API](../log-analytics/log-analytics-data-collector-api.md) 
| \*. ods.opinsights.azure.com | \*. ods.opinsights.azure.us | Agent komunikacji — [Konfigurowanie ustawień zapory](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. oms.opinsights.azure.com | \*. oms.opinsights.azure.us | Agent komunikacji — [Konfigurowanie ustawień zapory](../log-analytics/log-analytics-proxy-firewall.md) |
| \*. blob.core.windows.net | \*. blob.core.usgovcloudapi.net | Agent komunikacji — [Konfigurowanie ustawień zapory](../log-analytics/log-analytics-proxy-firewall.md) |


Następujące funkcje analizy dziennika mają różne zachowanie w Azure dla instytucji rządowych:

+ Agent systemu Windows musi zostać pobrany z [dziennika analizy portal](https://oms.microsoft.us) Azure administracji.
+ Aby połączyć serwera zarządzania System Center Operations Manager analizy dziennika, należy pobrać i importowanie zaktualizowanych pakietów zarządzania.
  1. Pobierz i Zapisz [zaktualizowany pakiety zarządzania](http://go.microsoft.com/fwlink/?LinkId=828749)
  2. Rozpakuj plik, który został pobrany
  3. Pakiety zarządzania zaimportować do programu Operations Manager. Aby uzyskać informacje na temat importowania pakiet zarządzania z dysku, zobacz temat [jak zaimportować pakiet zarządzania menedżera operacji](http://technet.microsoft.com/library/hh212691.aspx) w witrynie Microsoft TechNet w sieci Web.
  4. Aby połączyć programu Operations Manager analizy dziennika, postępuj zgodnie z instrukcjami w [Łączenie analizy dziennika programu Operations Manager](../log-analytics/log-analytics-om-agents.md) 



### <a name="frequently-asked-questions"></a>Często zadawane pytania

+ Czy mogę przenieść dane z dziennika analizy w publicznej dla instytucji rządowych Azure platformy Azure
  - Wartość nie. Nie jest możliwe przenoszenie danych lub obszaru roboczego z publicznej Azure do Azure dla instytucji rządowych.
+ Można przełączać się między publicznej Azure i dla instytucji rządowych Azure obszarów roboczych z portalu usługi OMS dziennika analizy?
  - Wartość nie. Portali publicznej Azure i dla instytucji rządowych Azure są oddzielone i nie udostępniaj informacji. 

Aby uzyskać więcej informacji dokumentacji [analizy dziennika publicznej](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Następne kroki

Informacje dodatkowe i aktualizacji subskrybowanie <a href="https://blogs.msdn.microsoft.com/azuregov/">Blog dotyczący programu Microsoft Azure dla instytucji rządowych.</a>
 
