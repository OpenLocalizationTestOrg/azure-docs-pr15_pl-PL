<properties
    pageTitle="Dzierżawa użycie zasobu interfejsu API | Microsoft Azure"
    description="Odwołania do użycia zasobów interfejsu API, co pobieranie informacji o zastosowania stos Azure."
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

# <a name="tenant-resource-usage-api"></a>Dzierżawa użycie zasobu interfejsu API

Dzierżawy umożliwia wyświetlanie danych użycia zasobów własnych dzierżawy interfejsu API dzierżawy. Ten interfejs API jest zgodna z interfejsu API zastosowania Azure (obecnie w podglądzie prywatnych).

Polecenia cmdlet programu Windows PowerShell **Get UsageAggregates** umożliwia uzyskiwanie zastosowania danych, takich jak Azure.

## <a name="api-call"></a>Interfejs API połączenia

### <a name="request"></a>Żądanie

Żądanie otrzymuje zużycie Szczegóły subskrypcji wymagane i wymagane przedziału czasu. Nie ma żadnych treści żądania.

| **Metoda**  | **Identyfikator URI żądania** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Pobierz         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenty

| **Argument**             | **Opis** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure punkt końcowy środowiska stos Azure Menedżera zasobów. |
| *subId*                   | Identyfikator subskrypcji użytkownika, który jest nawiązywanie połączenia. Za pomocą tego interfejsu API tylko do kwerendy użycia pojedynczej subskrypcji. Dostawców można użyć interfejsu API użycia zasobów dostawcy do użycie kwerendy dla wszystkich dzierżaw. |
| *reportedStartTime*       | Uruchamianie zapytania po raz. Wartość *DateTime* powinny być w czasie UTC i na początku godzinę, na przykład 13:00. Dzienny agregacji ta wartość północy czasu UTC. Format jest *wyjściowym* ISO 8601, na przykład 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, gdzie wyjściowym jest dwukropek do % 3a i plus wyjściowym do % 2b, tak aby była przyjazny identyfikatora URI. |
| *reportedEndTime*         | Czas zakończenia kwerendy. Ograniczenia, które dotyczą *reportedStartTime* dotyczą również ten argument. Wartość *reportedEndTime* nie może być w przyszłości. |
| *aggregationGranularity*  | Parametr opcjonalny, który ma dwie osobne wartości potencjalne: codziennie i co godzinę. Jako wartości wskazują, jedną zwraca dane w szczegółowości dzienny, a drugi jest rozdzielczość co godzinę. Opcja dzienna to ustawienie domyślne. |
| *subscriberId*            | Identyfikator subskrypcji. Aby uzyskać filtrowane dane, jest wymagany identyfikator subskrypcji bezpośredni dzierżawy dostawcy. Jeśli określono nie parametru identyfikator subskrypcji, połączenie zwraca danych dotyczących użycia dla wszystkich dostawcy bezpośredni dzierżaw. |
| *wersja API*             | Wersja protokołu, które jest używane do łączenia się to żądanie. Należy użyć 2015-06-01-Podgląd. |
| *continuationToken*       | Token pobierane z ostatniego połączenia dostawcy interfejsu API zastosowania. Jest to potrzebne, jeśli odpowiedź jest większa niż 1000 wierszy. Jest to zakładki w przypadku postępu. Jeśli nie istnieje, dane są pobierane od początku dnia lub przekazany godzinę, według szczegółowości. |

### <a name="response"></a>Odpowiedź

Pobierz /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = codziennie & wersja api = 1.0

{

"wartość":\[

{

"identyfikator": "-subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"Nazwa": "sub1 meterID1",

"typ": "Microsoft.Commerce/UsageAggregate"

"właściwości": {

"subscriptionId": "sub1"

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\" lokalizacji\\":\\" Alaska\\",\\" znaczniki\\": wartości null,\\" additionalInfo\\": wartości null}}",

:2.4000000000 "ilość"

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Szczegóły odpowiedzi

| **Argument**      | **Opis** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *Identyfikator*              | Unikatowy identyfikator agregacji zastosowania |
| *Nazwa*            | Nazwa agregacji zastosowania |
| *Typ*            | Definicja zasobów |
| *subscriptionId*  | Identyfikator subskrypcji Azure użytkownika |
| *usageStartTime*  | Czas kolorem zastosowania, do którego należy tego agregatu zastosowania rozpoczęcia UTC |
| *usageEndTime*    | Czas zakończenia UTC łańcucha zastosowania, do którego należy tego agregatu zastosowania |
| *instanceData*    | Pary klucz wartość Szczegóły wystąpienia (w nowym formacie):<br>  *resourceUri*: w pełni kwalifikowane identyfikator zasobu, w tym grupy zasobów oraz nazwę wystąpienia <br>  *Lokalizacja*: Region, w którym została uruchomiona usługa <br>  *znaczniki*: znaczniki zasobów, które użytkownik określi <br>  *additionalInfo*: więcej szczegółów o zasobie są używane, na przykład typ systemu operacyjnego wersji lub obrazu |
| *ilość*        | Ilość zużycie zasobów, który wystąpił w tym czasie |
| *meterId*         | Unikatowy identyfikator zasobu, który był zużyte (także nazywane *ResourceID*) |

## <a name="next-steps"></a>Następne kroki

[Często zadawane pytania dotyczące użycia](azure-stack-usage-related-faq.md)
