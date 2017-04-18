<properties
    pageTitle="Azure rozwiązanie klucza magazynu w dzienniku analizy | Microsoft Azure"
    description="Rozwiązanie Azure klucza magazynu w dzienniku analizy umożliwia przeglądanie dzienników Azure klucza magazynu."
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
    ms.date="07/12/2016"
    ms.author="richrund"/>

# <a name="azure-key-vault-preview-solution-in-log-analytics"></a>Azure rozwiązania klucza magazynu (wersja Preview) w programie analizy dziennika

>[AZURE.NOTE] Jest to [rozwiązanie Podgląd](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Rozwiązanie Azure klucza magazynu w dzienniku analizy umożliwia przeglądanie dzienników Azure klucza magazynu AuditEvent.

Możesz włączyć rejestrowanie zdarzeń inspekcji dla magazynu klucza Azure. Dzienniki te są zapisywane z magazynem obiektów Blob platformy Azure miejsce, w którym są mogą następnie być indeksowane przez analizy dziennika do wyszukiwania i analizy.

## <a name="install-and-configure-the-solution"></a>Instalowanie i konfigurowanie rozwiązania

Aby zainstalować i skonfigurować rozwiązanie Azure klucza magazynu, wykonaj następujące instrukcje:

1.  Włączanie [diagnostyki rejestrowanie dla magazynu klucz](../key-vault/key-vault-logging.md) zasoby, które chcesz monitorować
2.  Konfigurowanie analizy dziennika odczytać dzienniki z magazynem obiektów blob przy użyciu procesu opisanego w [plikach JSON w magazynie obiektów blob](../log-analytics/log-analytics-azure-storage-json.md).
3.  Włącz rozwiązanie Azure klucza magazynu przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  

## <a name="review-azure-key-vault-data-collection-details"></a>Przejrzyj szczegóły zbioru danych Azure klucza magazynu

Azure rozwiązanie klucza magazynu zbiera dzienniki diagnostyczne z magazynem obiektów blob Azure dla magazynu klucza Azure.
Agent nie jest wymagane na potrzeby zbierania danych.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane dla magazynu klucza Azure.

| Platformy | Bezpośrednie agenta | Systemy agenta menedżera operacji centrum (SCOM) | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | Częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Azure|![Brak](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Brak](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Tak](./media/log-analytics-azure-keyvault/oms-bullet-green.png)|            ![Brak](./media/log-analytics-azure-keyvault/oms-bullet-red.png)|![Brak](./media/log-analytics-azure-keyvault/oms-bullet-red.png)| 10 minut|

## <a name="use-azure-key-vault"></a>Używanie Azure klucza magazynu

Po zainstalowaniu rozwiązanie, możesz wyświetlić podsumowanie żądanie, które kafelka statusy czasie dla swojego monitorowane magazynami klucza przy użyciu **Magazynu klucza Azure** na stronie **Przegląd** analizy dziennika.

![Obraz kafelka Azure klucza magazynu](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Po kliknięciu kafelka **Przegląd** , można wyświetlać podsumowania dzienników i następnie przechodzić celu szczegóły dotyczące następujących kategorii:

- Głośność wszystkie operacje klucza magazynu w czasie
- Nie powiodło się wielkości operacji w czasie
- Średni czas oczekiwania operacyjne operacji
- Jakość usługi dla operacji z liczbę operacji przyjmujących więcej niż 1000 ms i lista działań, które pobierają więcej niż 1000 ms

![Obraz pulpitu nawigacyjnego Azure klucza magazynu](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Obraz pulpitu nawigacyjnego Azure klucza magazynu](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>Aby wyświetlić szczegóły dotyczące dowolnej operacji

1. Na stronie **Przegląd** kliknij **Azure klucza magazynu** .
2. Na pulpicie nawigacyjnym **Azure klucza magazynu** Sprawdź informacje w jednym z karty, a następnie kliknij jedną, aby wyświetlić szczegółowe informacje na stronie wyszukiwania dziennika.

    Na dowolnej stronie wyszukiwania dziennika możesz wyświetlić wyniki przez godzinę, szczegółowe wyniki i historii wyszukiwania dziennika. Możesz również filtrować według aspekty w celu zawężenia wyników.

## <a name="log-analytics-records"></a>Rekordy analizy dziennika

Rozwiązanie Azure klucza magazynu analizuje rekordy, które mają typ **KeyVaults** które są pobierane z [dzienników AuditEvent](../key-vault/key-vault-logging.md) w diagnostyki Azure.  Właściwości te rekordy znajdują się w poniższej tabeli.  

| Właściwość | Opis |
|:--|:--|
| Typ | *KeyVaults* |
| SourceSystem | *AzureStorage* |
| CallerIpAddress | Adres IP klienta, który żądanie |
| Kategoria      | Dzienniki magazynu klucza AuditEvent jest wartością jednym, dostępne.|
| CorrelationId | Opcjonalnie GUID, jaki klient może upłynąć do przeniesionym dzienniki po stronie klienta przy użyciu usługi rejestrowania po stronie (klawisz magazynu). |
| DurationMs | Czas potrzebny do obsługi żądania interfejsu API usługi REST, (w milisekundach). Nie dotyczy to czasu oczekiwania w sieci, więc podczas pomiaru po stronie klienta mogą być niezgodne z tym razem. |
| HttpStatusCode_d | Zwracane przez żądanie kodu stanu HTTP |
| Id_s       | Unikatowy identyfikator żądania |
| Identity_o | Tożsamość z tokenu, które zostały przedstawione podczas tworzenia żądania interfejsu API usługi REST. Jest to zazwyczaj "użytkownika", "wystawcy usługi" lub "użytkownika + identyfikator aplikacji" kombinacji tak jak w przypadku żądania wynikające z polecenia cmdlet programu PowerShell Azure. |
| OperationName      | Nazwa operacji opisane w [Azure klucza magazynu rejestrowania](../key-vault/key-vault-logging.md)|
| OperationVersion      | Wersja interfejsu API usługi REST żądany przez klienta|
| RemoteIPLatitude | Szerokość klienta, który żądanie |
| RemoteIPLongitude | Długość geograficzna klienta, który żądanie |
| RemoteIPCountry | Kraju klienta, który żądanie  |
| RequestUri_s | Identyfikator URI żądania |
| Zasób   | Nazwa klucza magazynu |
| Grupa zasobów | Grupa zasobów klucza magazynu |
| ResourceId | Identyfikator zasobu Azure Menedżera zasobów. W przypadku dzienników klucza magazynu jest zawsze identyfikator magazynu klucz zasobu. |
| ResourceProvider | *FIRMY MICROSOFT. KEYVAULT* |
| ResultSignature  | Stan HTTP|
| ResultType      | Wynik żądania interfejsu API usługi REST|
| SubscriptionId | Identyfikator subskrypcji Azure subskrypcji zawierającej magazynu klucza |


## <a name="next-steps"></a>Następne kroki

- Umożliwia wyświetlanie szczegółowych danych Azure klucza magazynu [dziennika wyszukiwania analizy dziennika](log-analytics-log-searches.md) .
