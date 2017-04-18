<properties
    pageTitle="Azure rozwiązania sieci analizy w programie analizy dziennika | Microsoft Azure"
    description="Za pomocą rozwiązanie analizy sieci Azure analizy dziennika umożliwia przeglądanie dzienników grupy zabezpieczeń sieci Azure i dzienniki Azure aplikacji bramy."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="richrund"/>

# <a name="azure-networking-analytics-preview-solution-in-log-analytics"></a>Azure rozwiązania sieci analizy (wersja Preview) w programie analizy dziennika

>[AZURE.NOTE] Jest to [rozwiązanie Podgląd](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Rozwiązanie Azure analizy sieci w dzienniku analizy umożliwia przeglądanie dzienników Azure aplikacji bramy i dzienniki grupy zabezpieczeń sieci Azure.

Możesz włączyć rejestrowanie dla dzienników Azure aplikacji bramy i grupy zabezpieczeń sieci Azure. Dzienniki te są zapisywane w Blob przestrzeni dyskowej, gdzie one można następnie być indeksowane przez analizy dziennika do wyszukiwania i analizy.

Dla bramy aplikacji są obsługiwane następujące dzienniki:

+ ApplicationGatewayAccessLog
+ ApplicationGatewayPerformanceLog

Dla grup zabezpieczeń sieci są obsługiwane następujące dzienniki:

+ NetworkSecurityGroupEvent
+ NetworkSecurityGroupRuleCounter

## <a name="install-and-configure-the-solution"></a>Instalowanie i konfigurowanie rozwiązania

Aby zainstalować i skonfigurować rozwiązanie Azure analizy sieci, wykonaj następujące instrukcje:

1.  Włącz rejestrowanie diagnostyczne dla zasobów, które chcesz monitorować:
  + [Brama aplikacji](../application-gateway/application-gateway-diagnostics.md)
  + [Grupa zabezpieczeń sieci](../virtual-network/virtual-network-nsg-manage-log.md)
2.  Konfigurowanie analizy dziennika odczytać dzienniki z magazynem obiektów Blob przy użyciu procesu opisanego w [plikach JSON w magazynie obiektów blob](../log-analytics/log-analytics-azure-storage-json.md).
3.  Włącz rozwiązanie Azure analizy sieci przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  

Jeśli rejestrowanie diagnostyczne dla określonego typu zasobów nie jest włączona, karty pulpitu nawigacyjnego dla tego zasobu jest puste.

## <a name="review-azure-networking-analytics-data-collection-details"></a>Przejrzyj szczegóły zbioru danych analizy sieci Azure

Rozwiązanie Azure analizy sieci zbiera dzienniki diagnostyczne z magazynem obiektów Blob platformy Azure Azure aplikacji bram i grupy zabezpieczeń sieci.
Agent nie jest wymagane na potrzeby zbierania danych.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące jak dane są zbierane dla Azure analizy sieci.

| Platformy | Bezpośrednie agenta | Systemy agenta menedżera operacji centrum (SCOM) | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | Częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Azure|![Brak](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Brak](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Tak](./media/log-analytics-azure-networking/oms-bullet-green.png)|            ![Brak](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Brak](./media/log-analytics-azure-networking/oms-bullet-red.png)| 10 minut|

## <a name="use-azure-networking-analytics"></a>Używanie Azure analizy sieci

Po zainstalowaniu rozwiązanie, możesz wyświetlić podsumowanie klienta i błędów serwera dla swojego monitorowane bram aplikacji przy użyciu **Analizy sieci Azure** kafelka na stronie **Przegląd** w dzienniku analizy.

![Obraz kafelka Azure analizy sieci](./media/log-analytics-azure-networking/log-analytics-azurenetworking-tile.png)

Po kliknięciu kafelka **Przegląd** , można wyświetlać podsumowania dzienników i następnie przechodzić celu szczegóły dotyczące następujących kategorii:

+ Dzienniki aplikacji bramy w programie Access
  - Błędy klienta i serwera bramy aplikacji programu access dzienników
  - Żądania na godzinę dla każdej bramy aplikacji
  - Dla każdej aplikacji bramy nie powiodło się żądania na godzinę
  - Błędy agenta użytkownika dla aplikacji bram
+ Wydajność bramy aplikacji
  - Kondycja hosta bramy aplikacji
  - Maksymalne i 95 percentylu dla żądania aplikacji bramy nie powiodło się
+ Grupa zabezpieczeń sieci zablokowane przepływów
  - Reguły grupy zabezpieczeń sieciowych z przepływami zablokowanych
  - Adresy MAC z przepływami zablokowanych
+ Grupa zabezpieczeń sieci dozwolone przepływów
  - Reguły grupy zabezpieczeń sieciowych z przepływami dozwolonych
  - Adresy MAC z przepływami dozwolonych


![Obraz pulpitu nawigacyjnego Azure analizy sieci](./media/log-analytics-azure-networking/log-analytics-azurenetworking01.png)

![Obraz pulpitu nawigacyjnego Azure analizy sieci](./media/log-analytics-azure-networking/log-analytics-azurenetworking02.png)

![Obraz pulpitu nawigacyjnego Azure analizy sieci](./media/log-analytics-azure-networking/log-analytics-azurenetworking03.png)

![Obraz pulpitu nawigacyjnego Azure analizy sieci](./media/log-analytics-azure-networking/log-analytics-azurenetworking04.png)

### <a name="to-view-details-for-any-log-summary"></a>Aby wyświetlić szczegóły dotyczące każdego podsumowania dziennika

1. Na stronie **Przegląd** kliknij **Azure analizy sieci** .
2. Na pulpicie nawigacyjnym **Azure analizy sieci** Sprawdź informacje w jednym z karty, a następnie kliknij jedną, aby wyświetlić szczegółowe informacje na stronie wyszukiwania dziennika.

    Na dowolnej stronie wyszukiwania dziennika możesz wyświetlić wyniki przez godzinę, szczegółowe wyniki i historii wyszukiwania dziennika. Możesz również filtrować według aspekty w celu zawężenia wyników.

## <a name="next-steps"></a>Następne kroki

- Umożliwia wyświetlanie szczegółowych danych analizy sieci Azure [wyszukiwania dziennika analizy dziennika](log-analytics-log-searches.md) .
