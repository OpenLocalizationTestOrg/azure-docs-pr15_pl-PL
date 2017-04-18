<properties
   pageTitle="Azure limity żądania Menedżera zasobów | Microsoft Azure"
   description="Informacje dotyczące używania ograniczania żądaniami Azure Menedżera zasobów, gdy Osiągnięto limity subskrypcji."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Ograniczanie żądań Menedżera zasobów

Dla każdej subskrypcji i dzierżawy ograniczeń Menedżera zasobów żądania 15 000 na godzinę do odczytu i zapisu żądania 1200 / godz. Jeśli aplikacji lub skrypt osiągnie te limity, należy ograniczyć wezwaniach na. W tym temacie przedstawiono sposób ustalania pozostałych żądań, których masz przed osiągnięciem limitu i jak ma reagować na osiągnięto limit.

Po osiągnięciu limitu, otrzymasz kod stanu HTTP **429 zbyt wiele żądań**.

Liczba żądań jest ograniczone do subskrypcji lub Twojej dzierżawy. Jeśli istnieje wiele aplikacjami wprowadzania żądania w ramach subskrypcji, żądania te aplikacje są dodawane razem w celu określenia liczby pozostałych żądań.

Żądania subskrypcji występujące są te dotyczą przechodzące identyfikator subskrypcji, takie jak pobieranie zasobu grupy w ramach subskrypcji. Żądania dzierżawy występujące nie powinno obejmować identyfikator subskrypcji, takie jak pobieranie prawidłowych lokalizacji Azure.

## <a name="remaining-requests"></a>Pozostałych żądań

Można określić liczbę pozostałych żądań, sprawdzając nagłówków odpowiedzi. Każdego żądania zawiera wartości dla liczby pozostałych odczytu i zapisu żądania. W poniższej tabeli opisano nagłówki odpowiedzi, które można sprawdzić dla tych wartości:

| Nagłówek odpowiedzi | Opis |
| --------------- | ----------- |
| x-MS-ratelimit-Remaining-Subscription-Reads | Subskrypcja występujące odczytuje pozostały |
| x-MS-ratelimit-Remaining-Subscription-Writes | Subskrypcja występujące zapisuje pozostała |
| x-MS-ratelimit-Remaining-tenant-Reads | Dzierżawy występujące odczytuje pozostały |
| x-MS-ratelimit-Remaining-tenant-Writes | Zapisuje dzierżawy występujące pozostała |
| x-MS-ratelimit-Remaining-Subscription-Resource-Requests | Subskrypcja ograniczone pozostała żądania typ zasobu.<br /><br />Ta wartość Nagłówek jest zwracany tylko wtedy, jeśli usługa została zastąpiona domyślny limit. Menedżer zasobów dodaje wartość zamiast subskrypcji odczytu lub zapisu. |
| x-MS-ratelimit-Remaining-Subscription-Resource-ENTITIES-Read | Subskrypcja ograniczone żądania zbioru typ zasobu pozostała.<br /><br />Ta wartość Nagłówek jest zwracany tylko wtedy, jeśli usługa została zastąpiona domyślny limit. Ta wartość zawiera liczbę pozostałych żądań zbierania (Lista zasobów). |
| x-MS-ratelimit-Remaining-tenant-Resource-Requests | Dzierżawy ograniczone pozostały żądania typ zasobu.<br /><br />Ten nagłówek jest dodawany tylko wezwań na poziomie dzierżawy i tylko wtedy, gdy usługa została zastąpiona domyślny limit. Menedżer zasobów dodaje wartość zamiast dzierżawy odczytu lub zapisu. |
| x-MS-ratelimit-Remaining-tenant-Resource-ENTITIES-Read | Dzierżawy ograniczone żądania zbioru typ zasobu pozostała.<br /><br />Ten nagłówek jest dodawany tylko wezwań na poziomie dzierżawy i tylko wtedy, gdy usługa została zastąpiona domyślny limit. |

## <a name="retrieving-the-header-values"></a>Pobieranie wartości nagłówka

Pobieranie te wartości nagłówka w skryptu ani kodu nie różni się od pobierania wartości nagłówka. 

Na przykład w kolumnie **C#**, pobierania wartość nagłówka z obiektu **HttpWebResponse** o nazwie **odpowiedzi** przy użyciu następującego kodu:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

W **programie PowerShell**można pobrać wartość nagłówka z operacji wywołaj WebRequest.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Lub, jeśli chcesz zobaczyć pozostałych żądań debugowania, można zapewnić **-Debugowanie** parametru polecenia cmdlet programu **PowerShell** .

    Get-AzureRmResourceGroup -Debug
    
Która zwraca dużą ilość informacji, łącznie z następującą wartością odpowiedzi:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

**Polecenie Azure**można pobrać wartość nagłówka za pomocą opcji więcej pełne.

    azure group list -vv --json

Która zwraca dużą ilość informacji, łącznie z następujących obiektów:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Oczekiwanie przed wysłaniem następnego żądania

Po osiągnięciu limitu żądań Menedżera zasobów zwraca kodu stanu **429** HTTP i **Ponów próbę po** wartości w nagłówku. **Ponów próbę po** wartość określa liczbę sekund, aplikacja powinna czekać (lub wstrzymania) przed wysłaniem następnego żądania. Wysłanie żądania przed upływem wartość ponów próbę żądania nie jest przetwarzana i zwracana jest nową wartość ponów próbę.
