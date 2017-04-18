<properties
   pageTitle="Jak używać Power BI osadzonego z pozostałych | Microsoft Azure"
   description="Dowiedz się, jak używać Power BI osadzonego z pozostałych "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>Jak używać Power BI osadzonego z pozostałych


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI osadzone: Czym jest i do czego służy
Omówienie Power BI osadzony opisanej w oficjalna [Witryna Power BI osadzony](https://azure.microsoft.com/services/power-bi-embedded/), ale Spójrzmy szybkie przed możemy przejść do szczegółowe informacje dotyczące korzystania z pozostałych.

Jest bardzo proste, naprawdę. Niezależny często chce używać wizualizacji danych dynamicznych usługi [Power](https://powerbi.microsoft.com) BI w ich aplikacji jako bloków konstrukcyjnych interfejsu użytkownika.

Ale znasz, osadzanie raportach usługi Power BI lub kafelków do strony sieci web jest już możliwe bez usługę Power BI osadzony Azure za pomocą **Interfejsu API programu Power BI**. Umożliwia udostępnianie raportów programu w tym samym organizacji, możesz osadzić raportów z uwierzytelnianiem Azure AD. Należy użytkownik, który wyświetla raportów Zaloguj się przy użyciu kont Azure AD. Umożliwia udostępnianie raportów programu dla wszystkich użytkowników (w tym użytkownikom zewnętrznym), po prostu można osadzić z dostęp anonimowy.

Zostanie wyświetlony ten prosty osadzanie rozwiązanie, ale raczej nie spełnia wymagań aplikacji model.
Większość aplikacji model potrzebne do świadczenia określonych danych dla własnych klientów, niekoniecznie użytkownicy w ich organizacji. Na przykład jeśli przedstawiania obsługę zarówno firma A i firma B, użytkownicy w firmie A należy tylko Zobacz dane do własnej firmy A. Oznacza to, że wielu dzierżawy jest wymagany dla dostarczania.

Model aplikacji może być również oferowanie własnej metody uwierzytelniania, takie jak uwierzytelnianie przy użyciu formularzy, uwierzytelnianie podstawowe itp.. Następnie osadzania rozwiązanie musi współpraca przy użyciu tej metody uwierzytelniania bezpieczne. Konieczne jest również dla użytkownicy będą mogli używać tych aplikacji model bez zakupu dodatkowego lub licencji z subskrypcji usługi Power BI.

 **Power BI osadzony** jest przeznaczony dla dokładnie tego rodzaju scenariusze model. Aby teraz gdy mamy już tego krótkie wprowadzenie do końca korzystaj w niektóre informacje

Możesz użyć .NET \(C#) lub SDK Node.js łatwe tworzenie aplikacji przy użyciu Power BI osadzony. Jednak w tym artykule będzie możemy wyjaśnić o przepływie HTTP \(z uwzględnieniem poziomach) usługi Power BI bez SDK. Opis ten przepływ, można utworzyć z aplikacji **z dowolnym językiem programowania**i można zrozumieć mocno istoty Power BI osadzony.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Tworzenie Power BI obszaru roboczego zbieranie i uzyskiwanie dostępu do klucza \(obsługi)
Power BI osadzony jest jednym z usług Azure. Tylko model osób używających Azure Portal jest naliczany opłat zastosowania \(co godzinę sesji użytkownika), a użytkownika, który wyświetla raport nie jest naliczany lub nawet wymaga subskrypcji usługi Azure.
Przed rozpoczęciem naszych opracowywania aplikacji, możemy utworzyć **zbioru roboczego Power BI** przy użyciu Azure Portal.

Każdego obszaru roboczego programu Power BI osadzony jest obszar roboczy dla każdego z klientów (dzierżawy) i można dodać wiele obszarów roboczych w każdym zbiorze obszaru roboczego. Ten sam klucz dostępu jest używane w poszczególnych zbiorów obszaru roboczego. W efekt kolekcja obszar roboczy jest granicę zabezpieczeń dla Power BI osadzony.

![](media\power-bi-embedded-iframe\create-workspace.png)

Po zakończeniu firma Microsoft, tworzenie kolekcji obszaru roboczego, skopiuj kod dostępu z Azure Portal.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] Możemy również Inicjowanie obsługi zbioru obszaru roboczego i uzyskiwanie klawisz dostępu za pośrednictwem interfejsu API usługi REST. Aby uzyskać więcej informacji, zobacz [Power BI zasobu dostawcy API](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Utwórz plik .pbix z Power BI Desktop
Dalej możemy utworzyć połączenie danych i raportów w celu osadzenia.
W przypadku tego zadania istnieje programowania lub kodu. Firma Microsoft korzysta tylko Power BI Desktop.
W tym artykule nie możemy przejść przez szczegółowe informacje dotyczące sposobu używania Power BI Desktop. Jeśli potrzebujesz pomocy w tym miejscu, zobacz [Wprowadzenie do Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). W naszym przykładzie użyjemy tylko [Próbkę Retail analizy](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Tworzenie obszaru roboczego programu Power BI

Teraz, gdy inicjowania obsługi administracyjnej wszystkich zakończeniu Zaczynamy Tworzenie obszaru roboczego klienta w zbiorze obszaru roboczego za pośrednictwem interfejsów API pozostałych. Następujące HTTP WPIS żądanie (pozostała) jest tworzenie nowego obszaru roboczego w naszym istniejącego zbioru obszaru roboczego. W naszym przykładzie nazwą kolekcji obszar roboczy jest **mypbiapp**.
Klawisz dostępu, które było wcześniej skopiowane, możemy po prostu ustaw jako **AppKey**. Jest bardzo proste uwierzytelniania!

**Żądanie HTTP**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**Odpowiedź HTTP**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Zwrócone **workspaceId** jest używana do następujących wezwań interfejsu API. Nasze aplikacji należy zachować tę wartość.

## <a name="import-pbix-file-into-the-workspace"></a>Importowanie pliku .pbix do obszaru roboczego
Każdego obszaru roboczego można udostępniać do jednego pliku Power BI Desktop z zestawu danych \(wraz z ustawieniami źródło danych) i raportów. Firma Microsoft można zaimportować plik naszych .pbix do obszaru roboczego, jak pokazano w poniższym kodzie. Jak widać, możemy przekazać pliku binarnego pliku .pbix przy użyciu MIME multipart HTTP.

Fragment uri **32960a09-6366-4208-a8bb-9e0678cdbb9d** jest workspaceId i kwerendy parametrycznej **datasetDisplayName** jest nazwę zestawu danych, aby utworzyć. Utworzonego zestawu danych zawiera wszystkie dane dotyczące artefaktów w .pbix plików, takie jako importowanych danych, wskaźnik w źródle danych itd.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

To zadanie importu może działać przez pewien czas. Po zakończeniu naszych aplikacji poprosić o stanie zadań przy użyciu identyfikatora importu. W naszym przykładzie identyfikator importu jest **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Poniższy prosi stanu przy użyciu tego identyfikatora importu:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Jeśli zadanie jest wykonane, odpowiedź HTTP może być tak jak poniżej:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Jeśli zadanie jest wykonane, odpowiedź HTTP może być więcej w następujący sposób:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Połączenia źródła danych \(i wielu dzierżawy danych)
Podczas niemal wszystkich artefaktów w pliku .pbix są importowane naszych obszaru roboczego, nie są poświadczenia dla źródła danych. W wyniku w trybie **zapytania bezpośredniego**osadzonego raportu nie mogą być wyświetlane nieprawidłowo. Jednak podczas korzystania z **trybu importu**, możemy wyświetlić raportu przy użyciu istniejącego zaimportowanych danych. W takim przypadku firma Microsoft Ustaw poświadczenia, wykonując poniższe czynności za pośrednictwem połączenia pozostałych.

Najpierw będziemy należy korzystać z bramy źródła danych. Firma Microsoft ustalić, czy zestaw danych **identyfikator** jest poprzednio zwróconych identyfikator.

**Żądanie HTTP**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**Odpowiedź HTTP**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Przy użyciu identyfikatora zwracane bramy i identyfikator źródła danych \(Zobacz poprzedniej **gatewayId** i **identyfikator** na zwracany wynik), firma Microsoft poświadczeń tego źródła danych można zmienić w następujący sposób:

**Żądanie HTTP**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**Odpowiedź HTTP**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

W produkcji możemy również skonfigurować parametry połączenia różnych dla każdego obszaru roboczego przy użyciu interfejsu API usługi REST. \(znaczy, można oddzielić bazy danych dla poszczególnych klientów.)

Zmienia się następujące parametry połączenia źródła danych za pomocą pozostałych.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Możesz też firma Microsoft korzysta z zabezpieczeń na poziomie wiersza w Power BI osadzone i można oddzielić dane dla poszczególnych użytkowników w jednym raporcie. As a result, umożliwia obsługę każdy raport klienta z tym samym .pbix \(interfejsu użytkownika itd.) i różnych źródeł danych.

> [AZURE.NOTE] Jeśli używasz **trybu importu** zamiast **trybie zapytania bezpośredniego**, istnieje sposobem odświeżanie modeli przy użyciu interfejsu API. I lokalnych źródeł danych za pomocą usługi Power BI bramy nie jest jeszcze obsługiwane w Power BI osadzony. Jednak naprawdę warto śledzą [blog usługi Power BI](https://powerbi.microsoft.com/blog/) dla nowości i o nadchodzących w przyszłych wersjach.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Uwierzytelnianie i hostingu (osadzania) raportów w strony sieci web

W poprzedniej interfejsu API usługi REST możemy użyć klawisz dostępu **AppKey** się jako nagłówek autoryzacji. Ponieważ te połączenia można rozwiązać po stronie serwera wewnętrznej bazy danych, jest bezpieczne.

Jednak podczas możemy osadzić raportu strony sieci web, ten rodzaj informacji o zabezpieczeniach będzie można rozwiązać za pomocą języka JavaScript \(serwera sieci Web). Następnie wartość nagłówek autoryzacji muszą być zabezpieczone. Nasze klawisz dostępu zostanie wykryta przez złośliwy użytkownik lub złośliwy kod, po wywołują wszystkie operacje przy użyciu tego klucza.

Firma Microsoft osadzić raportu strony sieci web, możemy należy używać obliczoną token zamiast klawisza dostępu **AppKey**. Nasze aplikacji należy utworzyć OAuth Json Web Token \(JWT) składające się z roszczeń i obliczoną podpisu cyfrowego. Jak pokazano poniżej, ten JWT OAuth jest tokeny rozdzielanych kropkami ciąg zakodowany.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Firma Microsoft najpierw należy przygotować wartości wejściowej został podpisany później. Ta wartość jest ciąg zakodowany w adresie url (rfc4648) base64 następujące json i są one rozdzielone kropka \(.) znak. Firma Microsoft będzie później, wyjaśniono, jak uzyskać identyfikator raportu.

> [AZURE.NOTE] Jeśli chcemy za pomocą wiersza poziom zabezpieczeń kontrola dostępu Power BI osadzony, możemy także określić **nazwę użytkownika** i **ról** w roszczeń.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Następnie należy utworzyć ciąg zakodowany base64 HMAC \(podpis) z algorytmu SHA256. Ta wartość wprowadzania podpisanego jest poprzedni ciąg.

Ostatnia, musi łączenie wprowadzania ciągu wartości i podpis za pomocą okresu \(.) znak. Ukończony ciąg jest token aplikacji osadzania raportu. Nawet jeśli token aplikacji zostanie wykryta przez złośliwego użytkownika, nie mogą uzyskać dostępu oryginalnego klucza dostępu. Ten token aplikacji wygaśnie szybko.

Oto przykład PHP następujące czynności:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Na koniec osadzanie raportu na stronę sieci web

Osadzania naszych raportu, możemy uzyskać adres url osadzania i **identyfikator** za pomocą następujących interfejsu API usługi REST raportu.

**Żądanie HTTP**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**Odpowiedź HTTP**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Firma Microsoft można osadzić raportu w naszym aplikacji sieci web przy użyciu poprzedniej tokenu aplikacji.
Jeśli przyjrzymy następnego kodu przykładowe byłego części jest taka sama, co w poprzednim przykładzie. W ostatniej części, w tym przykładzie pokazano **embedUrl** \(zobacz poprzedni wynik) w iframe i jest opublikowanie token aplikacji do elementu iframe.

> [AZURE.NOTE] Musisz zmienić wartość Identyfikator raportu na własny. Z powodu błędu w systemie zarządzania zawartością tag iframe w przykładzie kodu jest zapoznaj się też dosłownie. Usuwanie tekstu capped z znacznik, jeśli możesz skopiować i wkleić ten przykładowy kod.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

A Oto naszych wynik:

![](media\power-bi-embedded-iframe\view-report.png)

W tej chwili Power BI osadzony tylko zawiera raport w elementu iframe. Ale śledzą [Blog usługi Power BI](). Ulepszenia przyszłych można użyć nowego po stronie klienta interfejsów API, które będą Pozwól nam wysłać informacje do elementu iframe, a także otrzymywać. Atrakcyjne sprzętów!


## <a name="see-also"></a>Zobacz też
- [Uwierzytelniania i autoryzacji w Power BI osadzony](power-bi-embedded-app-token-flow.md)
