<properties
    pageTitle="Często zadawanych pytań dotyczących użycia | Microsoft Azure"
    description="Lista metr stos Azure, porównanie z interfejsu API zastosowania Azure wraz z godzinami zastosowania i zgłoszone czasu kodów błędów."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="azure-stack-usage-api-faqs"></a>Użycie interfejsu API Azure stos często zadawane pytania
Ten artykuł zawiera odpowiedzi na niektóre często zadawane pytania dotyczące interfejsu API zastosowania stos Azure.

## <a name="what-meter-ids-can-i-see"></a>Jakie miernik identyfikatorów może wyświetlać?

Obecnie zastosowania jest zgłoszone dla sieci, magazynowania i dostawców zasobów obliczeń.

| **Dostawcy zasobów** | **Miernik identyfikator** |**Nazwa licznika** | **Jednostki** | **Informacje dodatkowe** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Sieci** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Użycie adresów publiczny adres IP | Adres IP |                    
| **Miejsca do magazynowania**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*godzin | Pojemność zużywanej przez tabel |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*godzin | Pojemność zużywanej przez blob strony |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*godzin  | Pojemność zużywanej przez kolejki |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*godzin  | Pojemność zużywanej przez blob bloku |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Licznik żądania w 10,000s   | Żądania obsługi tabeli (w 10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Dane wchodzącego w GB | Tabela usług danych wchodzącego w GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress w GB | Tabela usługi dane wyjściowe w GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | Żądania zliczanie w 10,000s | Żądania obsługi BLOB (w 10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Dane wchodzącego w GB          | Obiektów blob usługi danych wchodzącego w GB 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress w GB              | Obiektów blob usługi dane wyjściowe w GB 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | Żądania zliczanie w 10,000s   | Żądania obsługi kolejki (w 10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Dane wchodzącego w GB         | Kolejka usługi danych wchodzącego w GB 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress w GB  | Kolejka usługi dane wyjściowe w GB 
| **Obliczenia** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | Godzin rozmiaru maszyn wirtualnych | Rozmiar maszyn wirtualnych |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Porównanie API zastosowania stos Azure [Azure zastosowania API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (obecnie w publicznej wersja preview)?

-   Interfejs API zastosowania dzierżawy jest zgodna z interfejsu API Azure, z jednym wyjątkiem: flagę *showDetails* obecnie nie jest obsługiwana w stos Azure.

-   Interfejs API zastosowania dostawcy dotyczy tylko usługi Azure stosu.

-   Obecnie [RateCard interfejsu API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) jest dostępna w Azure nie jest dostępna w stos Azure.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Jaka jest różnica między czasem zastosowania i czasu zgłoszone?

Raporty użycia danych mieć dwie wartości czasu głównym:

-   **Zgłoszony czas**. Jednostka czasu podczas wydarzenia zastosowania systemu zastosowania

-   **Czas użytkowania**. Podczas gdy została wykorzystana zasobów stos Azure

Rozbieżności w wartości mogą być widoczne dla zastosowania i godzinę zgłoszone dla zdarzenia określonego zastosowania. Opóźnienie może mieć dowolną długość jako wiele godziny w dowolnym środowisku.

Obecnie kwerendy można *tylko według czasu zgłoszone*.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Co oznaczają kodów błędów interfejs API zastosowania?

| **Kod stanu HTTP** | **Kod błędu** | **Opis** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| -400 — Nieprawidłowe żądanie        | *NoApiVersion*     | Brakuje parametru zapytania *wersja api* .
| -400 — Nieprawidłowe żądanie        | *InvalidProperty*  | Właściwość nie jest widoczny lub ma nieprawidłową wartość. Wiadomości w kodzie błędu w treści odpowiedzi służy do identyfikowania Brak właściwości.
| -400 — Nieprawidłowe żądanie        | *RequestEndTimeIsInFuture*  | Wartość *ReportedEndTime* jest w przyszłości. Wartości w przyszłości są niedozwolone dla tego argumentu.
| -400 — Nieprawidłowe żądanie        | *SubscriberIdIsNotDirectTenant*    | Połączenia dostawcy interfejsu API używany identyfikator subskrypcji, która nie jest prawidłową dzierżawy rozmówcy.
| -400 — Nieprawidłowe żądanie        | *SubscriptionIdMissingInRequest*   | Identyfikator subskrypcji rozmówcy nie jest widoczne.
| -400 — Nieprawidłowe żądanie        | *InvalidAggregationGranularity*   | Zażądano szczegółowości nieprawidłowe agregacji. Prawidłowe wartości to dzienne i co godzinę.
| 503                    | *ServiceUnavailable*   | Wystąpił błąd powtarzający operację, ponieważ usługa jest zajęty lub połączenie zostaje jest ograniczona. |

## <a name="next-steps"></a>Następne kroki
[Fakturowanie klientów i obciążenia zwrotnego w stos Azure](azure-stack-billing-and-chargeback.md)

[Użycie zasobu dostawcy interfejsu API](azure-stack-provider-resource-api.md)

[Dzierżawa użycie zasobu interfejsu API](azure-stack-tenant-resource-usage-api.md)
