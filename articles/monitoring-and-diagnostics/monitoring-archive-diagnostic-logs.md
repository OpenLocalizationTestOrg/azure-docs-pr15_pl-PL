<properties
    pageTitle="Archiwizowanie Azure dzienniki diagnostyczne | Microsoft Azure"
    description="Dowiedz się, jak archiwizowanie usługi Azure dzienników diagnostycznych dla długotrwałych przechowywania na koncie miejsca do magazynowania."
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
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Archiwizowanie Azure dzienniki diagnostyczne
W tym artykule pokażemy, jak można użyć portal Azure, polecenia cmdlet programu PowerShell, polecenie lub interfejsu API usługi REST do archiwizacji [Dzienniki diagnostyczne Azure](monitoring-overview-of-diagnostic-logs.md) na koncie miejsca do magazynowania. Ta opcja jest przydatne, jeśli chcesz zachować dzienniki diagnostyczne z zasad przechowywania opcjonalne inspekcji, analizy statyczną lub wykonywanie kopii zapasowych.

## <a name="prerequisites"></a>Wymagania wstępne
Zanim zaczniesz, musisz [utworzyć konto miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) , do którego można archiwizować dzienniki diagnostyczne. Zdecydowanie zaleca się nie używać istniejącego konta magazynu zawiera inne, — monitorowanie danych przechowywanych w nim, dzięki czemu można lepiej kontrolować dostęp do monitorowania danych. Jednak jeśli są również archiwizacji do dziennika aktywności i diagnostyczne metryki na koncie miejsca do magazynowania, może być zrozumiałe za pomocą tego konta miejsca do magazynowania dla dzienniki diagnostyczne także zachować wszystkie dane monitorowania w centralnej lokalizacji. Konto miejsca do magazynowania, którego używasz, musi być kontem miejsca do magazynowania uniwersalny, nie konto magazyn obiektów blob.

## <a name="diagnostic-settings"></a>Ustawienia diagnostyczne
Aby archiwizować dzienniki diagnostyczne za pomocą dowolnej z poniższych metod, możesz ustawić **Ustawienie diagnostycznych** dla określonego zasobu. Diagnostyczne ustawienie dla zasobu definiuje kategorie dzienniki, które są przechowywane lub strumieniowo i wydajność — Centrum konta i/lub wydarzenia miejsca do magazynowania. Definiuje również zasady przechowywania (liczba dni przechowywania) dla zdarzenia każdej kategorii dziennika przechowywane na koncie miejsca do magazynowania. Zasady przechowywania jest równa zero, zdarzeń dla tej kategorii dziennika są przechowywane czas nieokreślony (czyli zamieniana zawsze). Zasady przechowywania w przeciwnym razie może być dowolna liczba dni od 1 do 2147483647. [Można uzyskać więcej informacji o opisane tutaj ustawienia diagnostyczne](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>Dzienniki diagnostyczne archiwum za pomocą portalu

1. W portalu kliknij pozycję do karta zasobów dla zasobu, w którym chcesz włączyć archiwizacji: dzienniki diagnostyczne.
2. W sekcji **Monitorowanie** menu ustawień zasobu wybierz pozycję **Diagnostyka**.

    ![Monitorowanie sekcji menu zasobów](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. Zaznacz pole wyboru obok **Eksportowanie konta miejsca do magazynowania**, a następnie wybierz konto miejsca do magazynowania. Opcjonalnie, ustaw liczbę dni przechowywania dzienników przy użyciu **przechowywania (dni)** suwaki. Zatrzymywanie zero dni przechowuje dzienniki czas nieokreślony.

    ![Karta dzienniki diagnostyczne](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Kliknij przycisk **Zapisz**.

Dzienniki diagnostyczne są archiwizowane do tego konta miejsca do magazynowania, jak nowe dane zdarzenie jest generowany.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Dzienniki diagnostyczne archiwum przy użyciu poleceń cmdlet programu PowerShell

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Właściwość         | Wymagane | Opis                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceId       | Tak      | Identyfikator zasobu zasobu, dla którego chcesz ustawić ustawienia diagnostyczne.                            |
| StorageAccountId | Brak       | Identyfikator zasobu konta miejsca do magazynowania, do którego powinny być zapisywane dzienniki diagnostyczne.                          |
| Kategorie       | Brak       | Rozdzielaną średnikami listę kategorii dziennika, aby włączyć.                                                     |
| Włączone          | Tak      | Wartość logiczna wskazująca, czy diagnostyki są włączone lub wyłączone tego zasobu.                  |
| RetentionEnabled | Brak       | Wartość logiczna wskazująca, jeśli zasady przechowywania są włączone dla tego zasobu.                            |
| RetentionInDays  | Brak       | Liczba dni, dla których należy zachować zdarzenia od 1 do 2147483647. Wartość zero są przechowywane dzienniki czas nieokreślony. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>Archiwizowanie dziennika aktywności za pośrednictwem interfejsu wiersza polecenia i Platform

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Właściwość         | Wymagane | Opis                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| resourceId       | Tak      | Identyfikator zasobu zasobu, dla którego chcesz ustawić ustawienia diagnostyczne.                            |
| storageId        | Brak       | Identyfikator zasobu konta miejsca do magazynowania, do którego powinny być zapisywane dzienniki diagnostyczne.                          |
| kategorie       | Brak       | Rozdzielaną średnikami listę kategorii dziennika, aby włączyć.                                                     |
| włączone          | Tak      | Wartość logiczna wskazująca, czy diagnostyki są włączone lub wyłączone tego zasobu.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Dzienniki diagnostyczne archiwum za pośrednictwem interfejsu API usługi REST
[Zobacz ten dokument,](https://msdn.microsoft.com/library/azure/dn931931.aspx) Aby uzyskać informacje o sposobie konfigurowania ustawienia diagnostyczne za pomocą interfejsu API usługi REST Monitor Azure.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Schemat dzienniki diagnostyczne w oknie konta miejsca do magazynowania
Po zdefiniowaniu archiwizacji, kontenera magazynu jest tworzona w oknie konta miejsca do magazynowania zaraz po wystąpieniu zdarzenia w jednym z kategorii dziennika, które są włączone. BLOB w kontenerze wykonaj takim samym formatem różnych dzienniki diagnostyczne i dziennika aktywności. Struktura tych obiektów blob jest:

> wniosków - dzienniki-{nazwa kategorii dziennika}-resourceId = subskrypcje-{identyfikator subskrypcji} /RESOURCEGROUPS/ {Nazwa grupy zasobów} /PROVIDERS/ {Nazwa dostawcy zasobu} / {Typ zasobu}-{Nazwa zasobu}-y = {czterech cyfr roku liczbowe}-m = {dwóch cyfr liczbowe miesiąc} / d = {dwóch cyfr liczbowe dzień} / h = {hour}/m=00/PT1H.json dwóch cyfr 24-godzinnym

Lub po prostu,

> wniosków - dzienniki-{nazwa kategorii dziennika}-resourceId =-{zasób identyfikator}-y = {czterech cyfr roku liczbowe}-m = {dwóch cyfr liczbowe miesiąc} / d = {dwóch cyfr liczbowe dzień} / h = {hour}/m=00/PT1H.json dwóch cyfr 24-godzinnym

Na przykład nazwy obiektów blob mogą być:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Każdy obiektów blob PT1H.json zawiera blob JSON, zdarzeń, które wystąpiły w ciągu godziny określonego w adresie URL obiektów blob (na przykład h = 12). Podczas bieżącą godzinę zdarzeń zostaną dołączone do pliku PT1H.json występujące. Wartość minuty (m = 00) jest zawsze 00, ponieważ zdarzeń dziennika diagnostyczne są podzielone na poszczególnych obiektów blob na godzinę.

W pliku PT1H.json każde zdarzenie jest przechowywana w tablicy "rekordy", wykonując w tym formacie:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Nazwa elementu  | Opis                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| czas          | Sygnatura czasowa, gdy to zdarzenie zostało wygenerowane przez usługę Azure przetwarzania żądania odpowiednie zdarzenie. |
| resourceId    | Identyfikator zasobu ryzyko zasobu.                                                                       |
| operationName | Nazwa operacji.                                                                                      |
| Kategoria      | Kategoria dziennika zdarzeń.                                                                                  |
| właściwości    | Zestaw `<Key, Value>` par (to znaczy słownika) przedstawiający szczegóły zdarzenia.                            |

> [AZURE.NOTE] Właściwości i zastosowania tych właściwości mogą się różnić w zależności od tego zasobu.

## <a name="next-steps"></a>Następne kroki
- [Pobieranie obiektów blob na potrzeby analizy](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Dzienniki diagnostyczne strumienia do koncentratorów zdarzenia](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Dowiedz się więcej o dzienniki diagnostyczne](monitoring-overview-of-diagnostic-logs.md)
