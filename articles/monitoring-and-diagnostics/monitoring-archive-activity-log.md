<properties
    pageTitle="Archiwizowanie dziennika aktywności Azure | Microsoft Azure"
    description="Dowiedz się, jak Archiwizowanie dziennika aktywności usługi Azure długoterminowe przechowywania na koncie miejsca do magazynowania."
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
    ms.date="08/23/2016"
    ms.author="johnkem"/>

# <a name="archive-the-azure-activity-log"></a>Archiwizowanie dziennika aktywności Azure
W tym artykule pokażemy, jak można użyć portal Azure, poleceń cmdlet środowiska PowerShell lub interfejsu wiersza polecenia i Platform do archiwizacji [**Dziennik Azure**](monitoring-overview-activity-logs.md) na koncie miejsca do magazynowania. Ta opcja jest przydatne, jeśli chcesz zachować dłużej niż 90 dni (z pełną kontrolę nad zasady przechowywania) dla inspekcji, analizy statyczną lub kopia zapasowa dziennika aktywności. Jeśli musisz zachować zdarzenia 90 dni lub mniej nie musisz skonfigurować archiwizacji na konto miejsca do magazynowania, ponieważ dziennik zdarzeń są zachowywane w platformie Azure 90 dni bez włączania archiwizacji.

## <a name="prerequisites"></a>Wymagania wstępne
Zanim zaczniesz, musisz [utworzyć konto miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) , do którego można archiwizować dziennika aktywności. Zdecydowanie zaleca się nie używać istniejącego konta magazynu zawiera inne, — monitorowanie danych przechowywanych w nim, dzięki czemu można lepiej kontrolować dostęp do monitorowania danych. Jednak jeśli są również archiwizacji dzienniki diagnostyczne i wskaźniki na koncie miejsca do magazynowania, może być zrozumiałe za pomocą tego konta miejsca do magazynowania, a także dziennika aktywności zachować wszystkie dane monitorowania w centralnej lokalizacji. Konto miejsca do magazynowania, którego używasz, musi być kontem miejsca do magazynowania uniwersalny, nie konto magazyn obiektów blob.

## <a name="log-profile"></a>Profil dziennika
Aby archiwizować dziennik za pomocą dowolnej z poniższych metod, możesz ustawić **Dziennika profilu** dla subskrypcji. Profil dziennika określa rodzaj zdarzenia, które są przechowywane lub strumieniowo i wydajność — Centrum konta i/lub wydarzenia miejsca do magazynowania. Definiuje również zasady przechowywania (liczba dni przechowywania) dla zdarzeń przechowywane na koncie miejsca do magazynowania. Jeśli zasady przechowywania są ustawione na zero, zdarzenia są przechowywane czas nieokreślony. W przeciwnym razie można tak ustawić dowolną wartość z zakresu od 1 do 2147483647. [Można więcej informacji o dzienniku profilów tutaj](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles).

## <a name="archive-the-activity-log-using-the-portal"></a>Archiwizowanie dziennika aktywności za pomocą portalu
1. W portalu kliknij łącze **Dziennik** w obszarze nawigacji po lewej stronie. Jeśli nie widzisz łącza w dzienniku aktywności, najpierw kliknij łącze **Więcej usług** .

    ![Przejdź do karta dziennika aktywności](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. W górnej części karta kliknij przycisk **Eksportuj**.

    ![Kliknij przycisk Eksportuj](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. W kartę, która jest wyświetlana zaznacz pole wyboru obok **Eksportowanie konta miejsca do magazynowania** i wybierz konto miejsca do magazynowania.

    ![Ustawianie konta miejsca do magazynowania](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Przy użyciu suwaka lub pola tekstowego, określić liczbę dni, dla których powinny być zachowywane dziennik zdarzeń na koncie miejsca do magazynowania. Jeśli chcesz mieć danych zachowywane na koncie miejsca do magazynowania czas nieokreślony, należy ustawić ten numer na zero.
5. Kliknij przycisk **Zapisz**.

## <a name="archive-the-activity-log-via-powershell"></a>Archiwizowanie dziennika aktywności za pomocą programu PowerShell
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Właściwość         | Wymagane | Opis                                                                                                                                                                                                                                                                                       |
|------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StorageAccountId | Brak       | Identyfikator zasobu konta miejsca do magazynowania, do której mają być zapisywane dzienniki aktywności.                                                                                                                                                                                                                        |
| Lokalizacje        | Tak      | Rozdzielaną średnikami listę regionów, dla których chcesz zbierania zdarzeń dziennika aktywności. Można wyświetlić listę wszystkich regionów [, odwiedź witrynę tej strony](https://azure.microsoft.com/en-us/regions) lub za pomocą [Interfejsu API usługi REST zarządzania Azure](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| RetentionInDays  | Tak      | Liczba dni dla zdarzenia, które powinny zostać zachowane, od 1 do 2147483647. Wartość zero przechowuje dzienniki czas nieokreślony (zawsze).                                                                                                                                                                                             |
| Kategorie       | Tak      | Rozdzielaną średnikami listę kategorii zdarzeń, które powinny być pobierane. Możliwe wartości to zapisu, usuwania i akcji.                                                                                                                                                                                 |
## <a name="archive-the-activity-log-via-cli"></a>Archiwizowanie dziennika aktywności za pośrednictwem interfejsu wiersza polecenia
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Właściwość        | Wymagane | Opis                                                                                                                                                                                                                                                                                       |
|-----------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nazwa            | Tak      | Nazwa profilu dziennika.                                                                                                                                                                                                                                                                         |
| storageId       | Brak       | Identyfikator zasobu konta miejsca do magazynowania, do której mają być zapisywane dzienniki aktywności.                                                                                                                                                                                                                        |
| lokalizacje       | Tak      | Rozdzielaną średnikami listę regionów, dla których chcesz zbierania zdarzeń dziennika aktywności. Można wyświetlić listę wszystkich regionów [, odwiedź witrynę tej strony](https://azure.microsoft.com/en-us/regions) lub za pomocą [Interfejsu API usługi REST zarządzania Azure](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays | Tak      | Liczba dni dla zdarzenia, które powinny zostać zachowane, od 1 do 2147483647. Wartość zero będą przechowywane pliki dziennika czas nieokreślony (zawsze).                                                                                                                                                                                             |
| kategorie      | Tak      | Rozdzielaną średnikami listę kategorii zdarzeń, które powinny być pobierane. Możliwe wartości to zapisu, usuwania i akcji.                                                                                                                                                                                 |

## <a name="storage-schema-of-the-activity-log"></a>Schemat miejsca do magazynowania dziennika aktywności
Po zdefiniowaniu archiwizacji, kontenera magazynu zostanie utworzony na koncie miejsca do magazynowania zaraz po wystąpieniu zdarzenia dziennika aktywności. BLOB w kontenerze wykonaj takim samym formatem dziennika aktywności i dzienniki diagnostyczne. Struktura tych obiektów blob jest:

> wniosków operacyjne dzienniki/nazwa-= domyślny resourceId = subskrypcje-{identyfikator subskrypcji}-y = {czterech cyfr roku liczbowe}-m = {dwóch cyfr liczbowe miesiąc} / d = {dwóch cyfr liczbowe dzień} / h = {hour}/m=00/PT1H.json dwóch cyfr 24-godzinnym

Na przykład nazwy obiektów blob mogą być:

> insights-Operational-Logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON

Każdy obiektów blob PT1H.json zawiera blob JSON, zdarzeń, które wystąpiły w ciągu godziny określonego w adresie URL obiektów blob (np. h = 12). Podczas bieżącą godzinę zdarzeń zostaną dołączone do pliku PT1H.json występujące. Wartość minuty (m = 00) jest zawsze 00, ponieważ dziennik zdarzeń są podzielone na poszczególnych obiektów blob na godzinę.

W pliku PT1H.json każde zdarzenie jest przechowywana w tablicy "rekordy", wykonując w tym formacie:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Nazwa elementu    | Opis                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------|
| czas            | Sygnatura czasowa, gdy to zdarzenie zostało wygenerowane przez usługę Azure przetwarzania żądania odpowiednie zdarzenie.    |
| resourceId      | Identyfikator zasobu ryzyko zasobu.                                                                          |
| operationName   | Nazwa operacji.                                                                                         |
| Kategoria        | Kategoria działań, np. Zapis, Odczyt, akcji.                                                               |
| resultType      | Typ wyniku, np. Rozpocznij sukces, Niepowodzenie,                                                            |
| resultSignature | Zależy od typu zasobu.                                                                                  |
| durationMs      | Czas trwania operacji (w milisekundach)                                                                      |
| callerIpAddress | Adres IP użytkownika, który przeprowadził operacji, oświadczeń głównej nazwy użytkownika lub oświadczeniu SPN, na podstawie dostępności.         |
| correlationId   | Zazwyczaj GUID w postaci ciągu. Zdarzenia, które mają correlationId należą do tego samego działania uber.         |
| tożsamości        | Opisujące autoryzacji i roszczeń obiektów blob JSON.                                                             |
| Autoryzacja   | Obiektów blob RBAC właściwości zdarzenia. Zazwyczaj zawiera właściwości "działania", "roli" i "zakres".            |
| poziom           | Poziom zdarzenia. Jedną z następujących wartości: "Krytyczne", "Błąd", "Ostrzeżenia", "Informacyjne" i "Pełne" |
| Lokalizacja        | Region, w którym wystąpił lokalizację (lub globalny).                                                             |
| właściwości      | Zestaw `<Key, Value>` par (to znaczy słownika) przedstawiający szczegóły zdarzenia.                             |

> [AZURE.NOTE] Właściwości i zastosowania tych właściwości mogą się różnić w zależności od tego zasobu.

## <a name="next-steps"></a>Następne kroki
- [Pobieranie obiektów blob na potrzeby analizy](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Strumień dziennik do koncentratorów zdarzenia](monitoring-stream-activity-logs-event-hubs.md)
- [Dowiedz się więcej o dziennika aktywności](monitoring-overview-activity-logs.md)
