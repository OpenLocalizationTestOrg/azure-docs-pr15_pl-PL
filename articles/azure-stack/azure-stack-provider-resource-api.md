<properties
    pageTitle="Użycie zasobu dostawcy interfejsu API | Microsoft Azure"
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

# <a name="provider-resource-usage-api"></a>Użycie zasobu dostawcy interfejsu API

Dostawca terminów dotyczy administrator usługi i wszystkich dostawców delegowanej. Administratorów usług i dostawców delegowanej służy interfejsu API zastosowania dostawcy do wyświetlania zastosowania ich bezpośredni dzierżaw. Na przykład P0 można zadzwonić interfejsu API dostawcy Aby uzyskać informacje o zastosowania na osoby P1 i P2 i zastosowania bezpośredni i P1 można zadzwonić w celu zastosowania informacji na temat P3 i P4.

![Model koncepcyjny hierarchii dostawcy](media/azure-stack-provider-resource-api/image1.png)


## <a name="api-call-reference"></a>Interfejs API połączenia odwołania

### <a name="request"></a>Żądanie

Żądanie otrzymuje zużycie Szczegóły subskrypcji wymagane i wymagane przedziału czasu. Nie ma żadnych treści żądania.

Ten sposób użycia interfejsu API jest interfejs API dostawcy, więc wywołujący musi mieć przypisane roli właściciela, współautora lub czytnika dostawcy subskrypcji.

| **Metoda**  | **Identyfikator URI żądania** |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|  Pobierz        | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&subscriberId={sub1.1}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumenty

| **Argument**              | **Opis** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *armendpoint*             | Azure punkt końcowy środowiska stos Azure Menedżera zasobów. Konwencja stos Azure jest nazwa punktu końcowego ARM w format https://api. {Nazwa domeny} ". Na przykład jeśli nazwą domeny jest azurestack.local, punkt końcowy ARM będzie https://api.azurestack.local. |
| *subId*                   | Identyfikator subskrypcji użytkownika, który jest nawiązywanie połączenia. |
| *reportedStartTime*       | Uruchamianie zapytania po raz. Wartość *DateTime* powinny być w czasie UTC i na początku godzinę, na przykład 13:00. Dzienny agregacji ta wartość północy czasu UTC. Format jest *wyjściowym* ISO 8601, na przykład 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, gdzie wyjściowym jest dwukropek do % 3a i plus wyjściowym do % 2b, tak aby była przyjazny identyfikatora URI. |
| *reportedEndTime*         | Czas zakończenia kwerendy. Ograniczenia, które dotyczą *reportedStartTime* dotyczą również ten argument. Wartość *reportedEndTime* nie może być w przyszłości. |
| *aggregationGranularity*  | Parametr opcjonalny, który ma dwie osobne wartości potencjalne: codziennie i co godzinę. Jako wartości wskazują, jedną zwraca dane w szczegółowości dzienny, a drugi jest rozdzielczość co godzinę. Opcja dzienna to ustawienie domyślne. |
| *subscriberId*            | Identyfikator subskrypcji. Aby uzyskać filtrowane dane, jest wymagany identyfikator subskrypcji bezpośredni dzierżawy dostawcy. Jeśli określono nie parametru identyfikator subskrypcji, połączenie zwraca danych dotyczących użycia dla wszystkich dostawcy bezpośredni dzierżaw. |
| *wersja API*             | Wersja protokołu, które jest używane do łączenia się to żądanie. Należy użyć 2015-06-01-Podgląd. |
| *continuationToken*       | Token pobierane z ostatniego połączenia dostawcy interfejsu API zastosowania. Jest to potrzebne, jeśli odpowiedź jest większa niż 1000 wierszy. Jest to zakładki w przypadku postępu. Jeśli nie istnieje, dane są pobierane od początku dnia lub przekazany godzinę, według szczegółowości. |



### <a name="response"></a>Odpowiedź

Pobierz /subscriptions/sub1/providers/Microsoft.Commerce/subscriberUsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = codziennie & subscriberId = sub1.1 & wersja api = 1.0

{

"wartość":\[

{

"identyfikator": "/subscriptions/sub1.1/providers/Microsoft.Commerce/UsageAggregate/sub1.1-

meterID1 ",

"Nazwa": "sub1.1 meterID1",

"typ": "Microsoft.Commerce/UsageAggregate"

"właściwości": {

"subscriptionId": "sub1.1"

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\"lokalizacji\\

":\\" Alaska\\",\\" znaczniki\\": wartości null,\\" additionalInfo\\": wartości null}}",

:2.4000000000 "ilość"

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Szczegóły odpowiedzi


| **Argument**       | **Opis**
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *Identyfikator*               | Unikatowy identyfikator agregacji zastosowania
| *Nazwa*             | Nazwa agregacji zastosowania
| *Typ*             | Definicja zasobów
| *subscriptionId*   | Identyfikator subskrypcji użytkownika stos Azure
| *usageStartTime*   | Czas kolorem zastosowania, do którego należy tego agregatu zastosowania rozpoczęcia UTC
| *usageEndTime*     | Czas zakończenia UTC łańcucha zastosowania, do którego należy tego agregatu zastosowania
| *instanceData*     | Pary klucz wartość Szczegóły wystąpienia (w nowym formacie):<br> *resourceUri*: w pełni kwalifikowane identyfikator zasobu, który zawiera grupy zasobów oraz nazwę wystąpienia <br> *Lokalizacja*: Region, w którym została uruchomiona usługa <br> *znaczniki*: znaczniki zasobów, które są określone przez użytkownika <br> *additionalInfo*: więcej szczegółów o zasobie są używane, na przykład typ systemu operacyjnego wersji lub obrazu |
| *ilość*         | Ilość zużycie zasobów, który wystąpił w tym czasie |
| *meterId*          | Unikatowy identyfikator zasobu, który był zużyte (także nazywane *ResourceID*) |

## <a name="next-steps"></a>Następne kroki

[Użycie zasobu dzierżawy interfejsu API odwołania](azure-stack-tenant-resource-usage-api.md)

[Często zadawane pytania dotyczące użycia](azure-stack-usage-related-faq.md)
