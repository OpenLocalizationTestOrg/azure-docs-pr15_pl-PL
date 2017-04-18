<properties
    pageTitle="Zaloguj się analizy danych HTTP zbierających interfejsu API | Microsoft Azure"
    description="Za pomocą interfejsu API dziennika analizy HTTP danych zbierających JSON WPIS do wskazywania repozytorium analizy dziennika od dowolnego klienta, który można zadzwonić interfejsu API usługi REST. W tym artykule opisano, jak korzystać z interfejsu API i zawiera przykłady sposobu publikowania danych przy użyciu różnych językach programowania."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Dziennik analizy danych HTTP zbierających interfejsu API

Użycie interfejsu API Azure dziennika analizy HTTP danych zbierających dane, możesz dodać WPIS JavaScript obiektu notacji (JSON) danych do repozytorium analizy dziennika od dowolnego klienta, który można zadzwonić interfejsu API usługi REST. Przy użyciu tej metody, dane z aplikacji innych firm lub skrypty, takie jak można wysyłać z działań aranżacji w automatyzacji Azure.  

## <a name="create-a-request"></a>Utwórz żądanie

W następnych dwóch tabelach wymieniono atrybuty, które są wymagane dla każdego żądania API zbierających dziennika analizy HTTP danych. Opisano każdy atrybut bardziej szczegółowo w dalszej części tego artykułu.

### <a name="request-uri"></a>Identyfikator URI żądania

| Atrybut | Właściwość |
|:--|:--|
| Metoda | WPIS |
| IDENTYFIKATOR URI | https://\<Id_klienta\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Typ zawartości | Aplikacja json |

### <a name="request-uri-parameters"></a>Identyfikator URI parametrów żądania
| Parametr | Opis |
|:--|:--|
| Id_klienta  | Unikatowy identyfikator obszaru roboczego pakietu zarządzania Microsoft operacji. |
| Zasób    | Nazwa zasobu interfejsu API: / interfejsu api/dzienników. |
| Wersja API | Wersja interfejsu API do użytku z tego żądania. Obecnie jest 2016-04-01. |

### <a name="request-headers"></a>Żądanie nagłówków
| Nagłówek | Opis |
|:--|:--|
| Autoryzacja | Podpis autoryzacji. W dalszej części tego artykułu więcej o tworzeniu nagłówkiem HMAC SHA256. |
| Typ dziennika | Określ typ rekordu danych, który jest przesyłany. Obecnie typu dziennika obsługuje tylko alfa znaków. Nie obsługuje numeryczne ani znaków specjalnych. |
| x-ms-date | Data żądania została przetworzona w formacie RFC 1123. |
| wygenerowane — pole Time | Nazwa pola danych, które zawierają sygnatury czasowej elementu danych. Jeśli użytkownik określi polem jego zawartość są używane do **TimeGenerated**. Jeśli to pole nie jest określony, domyślną dla **TimeGenerated** jest czas, jaki jest spożywana wiadomości. Zawartość pola wiadomości należy wykonać formatu ISO 8601 YYYY-MM-DDThh:mm:ssZ. |


## <a name="authorization"></a>Autoryzacja

Dowolnego żądania API zbierających dziennika analizy HTTP danych musi zawierać nagłówek uwierzytelnienia. Aby uwierzytelnić żądanie, musisz zalogować się na żądanie podstawowy i klucz pomocnicza dla obszaru roboczego, który jest zgłaszającego żądanie. Następnie przekazanie tego podpisu jako część żądania.   

Poniżej przedstawiono format nagłówka autoryzacji:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* jest unikatowy identyfikator obszaru roboczego pakiet administracyjny operacji. *Podpis* jest [Oparty na procesie mieszania wiadomości uwierzytelniania kod (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , której jest tworzona z żądania, a następnie obliczona przy użyciu [algorytmu SHA256](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx). Następnie kodowania go w formacie Base64.

Użyj tego formatu do kodowania **SharedKey** ciąg podpisu:

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Oto przykład ciągu podpisu:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Jeśli ciąg podpisu, kodowania jej przy użyciu algorytmu HMAC SHA256 na ciąg zakodowany UTF-8, a następnie kodowanie wynik w postaci Base64. Użyj tego formatu:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Próbki w kolejnych sekcjach muszą przykładowy kod ułatwia tworzenie nagłówek uwierzytelnienia.

## <a name="request-body"></a>Treść żądania

Treść wiadomości musi być w formacie JSON. Musi zawierać co najmniej jeden rekord z pary nazwa i wartość właściwości w następującym formacie:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Wiele rekordów w pojedyncze żądanie można partii przy użyciu następującego formatu. Wszystkie rekordy musi być tego samego typu rekordu.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Typ rekordu i właściwości

Podczas przesyłania danych za pomocą interfejsu API dziennika analizy HTTP danych zbierających dane do definiowania typu rekordu niestandardowe. Obecnie nie można zapisywać danych istniejących typów rekordów, które zostały utworzone przez innych typów danych i rozwiązań. Analizy dziennika odczytuje przychodzące dane, a następnie tworzy właściwości, które są zgodne z typami danych wprowadzanych wartości.

Każdego żądania API analizy dziennika muszą zawierać **Typ dziennika** nagłówka z nazwę dla tego typu rekordu. Sufiks **_CL** jest automatycznie dołączane do nazwy wprowadź aby odróżnić go od innych typów dziennika niestandardowego logowania. Na przykład po wprowadzeniu nazwy **MyNewRecordType**analizy dziennika tworzy rekord z typem **MyNewRecordType_CL**. Dzięki temu, upewnij się, że nie występują żadne konflikty między utworzone przez użytkownika wpisz nazwy i zostały zrealizowane w bieżącej lub przyszłe rozwiązania Microsoft.

Aby określić typ danych właściwości, dziennika analizę dodaje sufiks do nazwy właściwości. Jeśli właściwość zawiera wartość null, właściwość jest niedostępna w tym rekordzie. W poniższej tabeli wymieniono typ danych właściwości i odpowiadające im sufiksu:

| Typ danych właściwości | Sufiks |
|:--|:--|
| Ciąg    | _s |
| Wartość logiczna   | _b |
| Naciśnij dwukrotnie    | _d |
| Data/Godzina | _P |
| IDENTYFIKATOR GUID      | _g |


Typ danych, który używa dziennika analizy dla każdej właściwości zależy od tego, czy już istnieje typ rekordu dla nowego rekordu.

- Jeśli typem rekordu nie istnieje, analizy dziennika tworzy nową. Analizy dziennika używa wnioskowanie typ JSON, aby określić typ danych dla każdej właściwości dla nowego rekordu.
- Jeśli istnieje typ rekordu, analizy dziennika próbuje utworzyć nowy rekord na podstawie istniejącej właściwości. Jeśli typ danych dla właściwości w nowym rekordzie nie są zgodne z i nie będzie można przekonwertować istniejący typ lub rekord zawiera właściwość, która nie istnieje, analizy dziennika tworzy nową właściwość, którą odpowiednich sufiks.

Na przykład ten wpis przesyłania czy Utwórz rekord z trzech właściwości, **number_d** **boolean_b**i **string_s**:

![Przykładowy rekord 1](media/log-analytics-data-collector-api/record-01.png)

Jeśli następnie przesłane zapisem następnego z wszystkich wartości sformatowane jako ciągi, właściwości nie chcesz zmienić. Te wartości można konwertować istniejących typów danych:

![Przykładowy rekord 2](media/log-analytics-data-collector-api/record-02.png)

Jednak jeśli utworzono następnie ponowne przesłanie dalej, analizy dziennika utworzyć nowe właściwości **boolean_d** i **string_d**. Nie można przekonwertować tych wartości:

![Przykładowy rekord 3](media/log-analytics-data-collector-api/record-03.png)

Jeśli przed utworzeniem tego typu rekordu, następnie przesłane następujący wpis, analizy dziennika czy utworzyć rekord z trzech właściwości, **liczba_s**, **boolean_s**i **string_s**. W tym wpisie wartości początkowej, zostało sformatowane jako ciąg:

![Przykładowy rekord 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Limity danych
Istnieją pewne ograniczenia wokół dane publikowane w zbiorze danych analizy dziennika interfejsu API.

- Maksymalnie 30 MB na wpis dziennika analizy danych zbierających API. To jest limit rozmiaru pojedynczy wpis. Jeśli dane z jednym wpisem przekracza 30 MB, należy podzielić dane do mniejszych wymiarach fragmentów i wysłać je jednocześnie. 
- Maksymalny limit 32 KB dla wartości pola. Jeśli wartość pola jest większa niż 32 KB, danych zostanie obcięta. 
- Zalecana maksymalna liczba pól dla danego typu jest 50. Jest to ograniczenie praktyczne z użyteczności i perspektywy obsługi wyszukiwania.  


## <a name="return-codes"></a>Kody zwrotne

Kod stanu HTTP 202 oznacza, że żądanie zostało zaakceptowane do przetwarzania, ale przetwarzanie nie zostało jeszcze zakończone. Oznacza to, że operacja ukończona pomyślnie.

W poniższej tabeli wymieniono kompletny zestaw kodów stanu, które mogą zwracać usługi:

| Kod | Stan | Kod błędu | Opis |
|:--|:--|:--|:--|
| 202 | Zaakceptowane |  | Żądanie zostało pomyślnie zaakceptowane. |
| 400 | Nieprawidłowe żądanie | InactiveCustomer | Obszar roboczy został zamknięty. |
| 400 | Nieprawidłowe żądanie | InvalidApiVersion | Wersja API, określony nie został rozpoznany przez usługę. |
| 400 | Nieprawidłowe żądanie | InvalidCustomerId | Określony identyfikator obszaru roboczego jest nieprawidłowy. |
| 400 | Nieprawidłowe żądanie | InvalidDataFormat | Dostarczono nieprawidłowe JSON. Treść odpowiedzi może zawierać więcej informacji na temat rozwiązywania błędów. |
| 400 | Nieprawidłowe żądanie | InvalidLogType | Typ dziennika określony zawartych znaki specjalne lub numeryczne. |
| 400 | Nieprawidłowe żądanie | MissingApiVersion | Wersja API nie określono. |
| 400 | Nieprawidłowe żądanie | MissingContentType | Typ zawartości nie zostało określone. |
| 400 | Nieprawidłowe żądanie | MissingLogType | Nie określono typu dziennika żądaną wartość. |
| 400 | Nieprawidłowe żądanie | UnsupportedContentType | Typ zawartości nie została ustawiona do **aplikacji i json**. |
| 403 | Dostęp zabroniony | InvalidAuthorization | Nie powiodło się uwierzytelnienie żądanie usługi. Sprawdź, czy klucz połączenia i identyfikator obszaru roboczego są prawidłowe. |
| 500 | Wewnętrzny błąd serwera | UnspecifiedError | Usługa wystąpił błąd wewnętrzny. Ponów próbę żądania. |
| 503 | Usługa jest niedostępna | ServiceUnavailable | Obecnie usługa jest niedostępna na odbieranie żądań. Ponów próbę żądania. |

## <a name="query-data"></a>Dane kwerendy

Kwerendy danych przedstawionych przez interfejsu API dziennika analizy HTTP danych zbierających, wyszukiwanie rekordów **typu** jest równa wartości **LogType** istnieje określonego, dołączonych z **_CL**. Na przykład jeśli użyto **MyCustomLog**, następnie możesz zwraca wszystkie rekordy zawierające **Typ = MyCustomLog_CL**.


## <a name="sample-requests"></a>Prośby o przykłady

W kolejnych sekcjach znajdują się przykłady sposobów przesyłanie danych do interfejsu API dziennika analizy HTTP danych zbierających dane przy użyciu różnych językach programowania.

Dla każdej próbki wykonaj poniższe czynności, aby ustawić zmienne nagłówka autoryzacji:

1. W portalu pakiet administracyjny operacji wybierz kafelka **Ustawienia** , a następnie wybierz kartę **Połączonych źródeł** .
2. Po prawej stronie **Identyfikator obszaru roboczego**wybierz ikonę Kopiuj, a następnie wklej identyfikator jako wartość zmiennej **Identyfikator klienta** .
3. Po prawej stronie **Klucz podstawowy**wybierz ikonę Kopiuj, a następnie wklej identyfikator jako wartość zmiennej **Klucz udostępniony** .

Można również zmienić zmienne typu dziennika i danych JSON.

### <a name="powershell-sample"></a>Przykładowy programu PowerShell

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>Przykładowe C#

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Przykładowe Python

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Następne kroki

- Umożliwia tworzenie niestandardowych widoków na dane, które można przesłać [Projektant widoków](log-analytics-view-designer.md) .
