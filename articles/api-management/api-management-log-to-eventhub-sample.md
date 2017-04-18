<properties
    pageTitle="Monitorowanie usługi interfejsy API zarządzania Azure interfejsu API, koncentratory zdarzenia i Runscope"
    description="Przykładowa aplikacja pokazujące zasad dziennika do eventhub łączących zarządzania interfejsu API Azure, koncentratory zdarzenia Azure i Runscope dla protokołu HTTP rejestrowania i monitorowania"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Monitorowanie usługi interfejsy API zarządzania Azure interfejsu API, koncentratory zdarzenia i Runscope

[Usługa zarządzania interfejsu API](api-management-key-concepts.md) udostępnia wiele możliwości przetwarzania żądania HTTP wysyłane do interfejsu API usługi HTTP. Jednak istnienia zaproszenia i odpowiedzi są przejściowych. Wniosek i przepływał przez usługę Zarządzanie interfejsu API do swojego interfejsu API wewnętrznej bazy danych. Z interfejsu API przetwarza żądanie i odpowiedź przepływał wstecz przez konsumenta interfejsu API. Usługi zarządzania interfejsu API zachowuje niektóre ważne statystykę za pośrednictwem interfejsów API do wyświetlania w programu Publisher, portalu pulpitu nawigacyjnego w ale poza, że dane są dostępne.

Za pomocą [dziennika do eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [zasady](api-management-howto-policies.md) w usłudze zarządzania interfejsu API możesz wysłać wszystkie szczegóły z i odpowiadania na wezwania do [Centrum zdarzeń Azure](../event-hubs/event-hubs-what-is-event-hubs.md). Istnieje wiele powodów, dlaczego warto generują zdarzenia od HTTP wiadomości wysyłane do Twojej interfejsów API. Przykłady dziennika inspekcji aktualizacje, analizy użycia, wyjątku alertów i 3 integracji innych firm.   

W tym artykule przedstawiono sposób rejestrowania całą wiadomość i odpowiadania na wezwania HTTP, wyślij go do koncentratora zdarzeń i następnie przekazywania wiadomości do innej strony usługa, która zapewnia HTTP rejestrowania i monitorowania usług.

## <a name="why-send-from-api-management-service"></a>Dlaczego wysyłać z interfejsu API usługi zarządzania?
Istnieje możliwość tworzenia oprogramowanie pośrednie HTTP, które można podłączyć do RAM HTTP interfejsu API do przechwytywania żądań i odpowiedzi HTTP i ich docierać do rejestrowania i monitorowania systemów. Wadą podwyższonego tego rozwiązania jest pośredniczącym HTTP musi być zintegrowany interfejs API wewnętrznej bazy danych i musi odpowiadać platformy interfejsu API. Jeśli istnieje wiele interfejsów API każdego z nich należy wdrożyć pośredniczącym. Często są powodów, dlaczego nie można zaktualizować interfejsów API w wewnętrznej bazie danych.

Integracja z infrastruktury rejestrowania za pomocą usługi zarządzania interfejsu API Azure rozwiązaniem jest scentralizowane i niezależne od platformy. Jest również skalowalna w części ze względu na możliwości [replikacji geo](api-management-howto-deploy-multi-region.md) zarządzania interfejsu API Azure.

## <a name="why-send-to-an-azure-event-hub"></a>Dlaczego wysłać do koncentratora zdarzenia Azure?
Jest rozsądne pytanie, dlaczego warto tworzyć zasady dotyczące koncentratorów zdarzenia Azure? Istnieje wiele różnych miejsc, w której może być logowania moich wezwań. Dlaczego nie tylko wysłać żądania bezpośrednio do miejsca docelowego?  To jest odpowiednią opcję. Po wprowadzeniu żądania rejestrowania z interfejsu API usługi zarządzania, należy brać pod uwagę jak wiadomości rejestrowania będzie wpływ na wydajność interfejsu API. Stopniowe zwiększenie obciążenia można rozwiązać, zwiększając dostępne wystąpienia składników systemu lub korzystając z replikacji geo. Jednak krótki tych najwyższych wartościach w ruchu mogą powodować żądania znacznie opóźnione żądania infrastruktury rejestrowania uruchomić spowalniać obciążeniu.

Koncentratory zdarzenia Azure służy do ingress dużej ilości danych, z możliwości zajmujących się znacznie większa niż liczba zdarzeń niż liczba HTTP żądań większość proces interfejsów API. Centrum zdarzenie działa jako rodzaj zaawansowanych bufora pomiędzy interfejsu API usługi zarządzania i infrastrukturę, która będzie przechowywanie i przetwarzanie wiadomości. Dzięki temu, że wydajność interfejsu API nie będą występować z powodu infrastruktury rejestrowania.  

Gdy dane przekazane do koncentratora zdarzenie jest zachowywane i będzie czekać na konsumentów zdarzenia centrum przetwarzania go. Centrum zdarzenie nie zwracać sposobu przetwarzania, ją po prostu szanuje zagwarantować, że wiadomość zostanie dostarczona pomyślnie.     

Koncentratory wydarzenia mają możliwość strumienia zdarzeń do wielu grup dla klientów indywidualnych. Dzięki temu zdarzeń do przetworzenia przez całkowicie systemów. Dzięki temu obsługi wielu scenariuszy integracji bez umieszczania dodanie opóźnienia w przetwarzania żądania interfejsu API w ramach usługi zarządzania interfejsu API do wygenerowania potrzeb tylko jedno zdarzenie.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Zasady do wysyłania wiadomości aplikację/http
Centrum zdarzeń wprowadzania danych zdarzenia prosty ciąg znaków. Zawartość tego ciągu są całkowicie dowolne. Aby można było grupowanie żądania HTTP i wysyłanie go do koncentratorów zdarzenia należy sformatować ciąg z informacjami żądania lub odpowiedzi. W sytuacjach tak Jeśli istnieje istniejącego formatu możemy ponowne używanie, a następnie może nie być napisać własną analizowania kodu. Początkowo I uważany za pomocą [HAR](http://www.softwareishard.com/blog/har-12-spec/) do wysyłania żądań i odpowiedzi HTTP. Jednak w tym formacie jest zoptymalizowana pod kątem przechowywania sekwencji żądania HTTP w formacie JSON podstawie. Zawiera liczbę obowiązkowe elementy dodane niepotrzebne złożoności dla tego scenariusza przekazywania wiadomości HTTP przez sieć.  

Opcja alternatywna było za pomocą `application/http` typ multimediów, zgodnie z opisem w specyfikacji HTTP [RFC 7230](http://tools.ietf.org/html/rfc7230). Ten typ multimediów używa dokładnie takim samym formatem używanym do rzeczywistego wysyłania wiadomości HTTP przez sieć, ale cały komunikat można umieścić w treści innego żądania HTTP. W naszym przypadku tylko chwilę firmę treści wiadomości do wysłania do koncentratorów zdarzenia. Wygodnie jest parsera, która istnieje w bibliotekach [Interfejsu API 2.2 klient programu Microsoft ASP.NET sieci Web](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) , których można przeanalizować tego formatu i konwertować macierzystych `HttpRequestMessage` i `HttpResponseMessage` obiektów.

Aby można było utworzyć tę wiadomość, którą należy skorzystać z C# podstawie [zasad wyrażeń](https://msdn.microsoft.com/library/azure/dn910913.aspx) w zarządzaniu interfejsu API Azure. Poniżej przedstawiono zasady, którego wysyła wiadomość żądania HTTP do koncentratorów zdarzenia Azure.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Deklaracji zasad
Istnieje kilka rzeczy określonego warte tworzenie wzmianki o o to wyrażenie zasad. Zasady dziennika do eventhub ma atrybut o nazwie identyfikator rejestratora, która odwołuje się do nazwy rejestratora utworzonym w ramach usługi zarządzania interfejsu API. Szczegóły dotyczące konfiguracji rejestratora Centrum zdarzeń w usłudze Zarządzanie interfejsu API można znaleźć w dokumencie [sposobu rejestrowania zdarzeń do koncentratorów zdarzenia Azure w zarządzaniu interfejsu API Azure](api-management-howto-log-event-hubs.md). Druga atrybut jest parametr opcjonalny, który powoduje, że koncentratory zdarzenia, które partycje do przechowywania wiadomości w. Koncentratory zdarzenia partycje za pomocą włączyć scalabilty i wymaga co najmniej dwóch. Uporządkowane dostarczanie wiadomości tylko jest gwarantowana we partycją. Jeśli firma Microsoft nie polecić Centrum zdarzenia, w których partition, aby umieścić wiadomości użyje algorytmu karuzeli do dystrybucji obciążenia. Jednak mogą powodować niektóre z naszych wiadomości mają być przetwarzane w kolejności.  

### <a name="partitions"></a>Partycje
W celu zapewnienia naszych wiadomości są dostarczane do odbiorcy w kolejności i wykorzystać możliwości dystrybucji obciążenia partycje, wybrano do wysyłania wiadomości żądania HTTP jedną partycją i HTTP wiadomości odpowiedzi do drugiego partycją. Dzięki dystrybucji obciążenia nawet i możemy zagwarantować, że wszystkie żądania zostaną wykorzystane w kolejności i wszystkie odpowiedzi zostaną wykorzystane w kolejności. Istnieje możliwość odpowiedź do zużycia przed odpowiednie żądanie, ale, który nie jest problem podczas mamy różnych mechanizm korelacji żądania do odpowiedzi i będziemy wiedzieć, że żądania zawsze poprzedzające odpowiedzi.

### <a name="http-payloads"></a>Ładunki HTTP
Po konstrukcyjnych `requestLine` możemy Sprawdź, jeśli treści żądania należy takiej liczby obcinany. Treść żądania zostanie obcięta do 1024 tylko. To można zwiększyć, jednak pojedyncze wiadomości Centrum wydarzenia są ograniczone do 256KB, dlatego prawdopodobieństwo, że niektóre wiadomości HTTP organów będzie nie mieści się w pojedynczej wiadomości. Podczas rejestrowania i analizy dużo informacji mogą pochodzić z tylko wiersz żądania HTTP i nagłówki. Ponadto wiele żądań interfejsu API zwrócić tylko małych organów a więc utratę wartość informacji przez Obcinanie dużych organów dość minimalnego w porównaniu z systemem zmniejszenie przełączania, przetwarzania i koszty magazynowania zachować wszystkie treść. Ostateczne notatkami dotyczący przetwarzania treści jest, które administrator powinien przekazać `true` na tym, co<string>metody (), ponieważ możemy czytania treść, ale została również ma interfejsu API, aby można było odczytać treści wewnętrznej bazy danych. Przez przekazywanie wartość PRAWDA, aby ta metoda możemy powodować treści, aby buforowane, tak aby można ją odczytać po raz drugi. To jest należy pamiętać: Jeśli masz interfejs API, który nie przekazywania bardzo dużych plików ani nie używa długie ankieta. W takich przypadkach może być unikanie treści do czytania w ogóle.   

### <a name="http-headers"></a>Nagłówki HTTP
Nagłówki HTTP może po prostu przenoszone do formatu wiadomości w formacie pary prosty klucza/wartości. Firma Microsoft wybrano odłączenia niektóre zabezpieczeń pola poufnych, aby uniknąć niepotrzebnego przeciek informacjami o poświadczeniach. Prawdopodobnie, że interfejs API klucze i inne poświadczenia będzie używane na potrzeby analizy. Jeśli firma Microsoft chcesz wykonać analizy na użytkownika i określonego produktu używają, a następnie firma Microsoft można pobrać z `context` obiektu i dodać te informacje do wiadomości.     
### <a name="message-metadata"></a>Komunikatów metadanych
Tworząc pełną wiadomość do wysłania do Centrum zdarzenia, pierwszy wiersz nie jest faktycznie część `application/http` wiadomości. Pierwszy wiersz jest dodatkowe metadane składający się z tego, czy wiadomość jest żądanie lub odpowiedź i identyfikator wiadomości, który jest używany do przeniesionym żądania do odpowiedzi. Identyfikator wiadomości jest tworzona przy użyciu innego zasad, która wygląda następująco:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

Firma Microsoft może utworzyć wiadomości z wezwaniem, który przechowywane w zmiennej do momentu odpowiedzi była zwracana, a następnie po prostu wysyłane żądania i odpowiedzi jako pojedynczej wiadomości. Jednak niezależnie wysłanie żądania i odpowiedzi i przeniesionym dwóch za pomocą identyfikatora wiadomości, będziemy korzystać z nieco bardziej elastyczne rozmiaru wiadomości, możliwość korzystać z wielu partycje przy jednoczesnym utrzymania porządku wiadomości i żądania pojawi się na pulpicie nawigacyjnym naszych rejestrowanie wcześniej. Może istnieć kilka scenariuszy, w których prawidłowa odpowiedź nigdy nie są wysyłane do koncentratora zdarzenia, prawdopodobnie ze względu na błąd krytyczny żądania usługi zarządzania interfejsu API, ale istnieją będzie jeszcze rekord żądania.

Zasady, aby wysłać odpowiedź HTTP jest bardzo podobne do wezwania i dlatego konfiguracja zasad wykonania wygląda następująco:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

`set-variable` Zasady tworzy wartość, która jest dostępna dla obu `log-to-eventhub` zasad w `<inbound>` sekcji i `<outbound>` sekcji.  

## <a name="receiving-events-from-event-hubs"></a>Odbieranie zdarzeń z koncentratorów zdarzenia
Wydarzenia z Centrum zdarzeń Azure są odbierane przy użyciu [protokołu AMQP](http://www.amqp.org/). Zespół Bus usługi Microsoft dokonano klienta bibliotek dostępne ułatwić dużo zdarzeń. Istnieją dwie metody różnych obsługiwane, jest jeden *Bezpośredni dla klientów indywidualnych* i drugi przy użyciu `EventProcessorHost` zajęć. Przykłady tych dwóch metod można znaleźć w [Zdarzenia koncentratory Przewodnik po programach](../event-hubs/event-hubs-programming-guide.md). Jest krótka wersja różnic między nimi, `Direct Consumer` zapewnia pełną kontrolę oraz `EventProcessorHost` czy część pracy instalacji wodociągowych dla ale powoduje pewne założenia dotyczące sposobu będzie przetwarzał tych zdarzeń.  

### <a name="eventprocessorhost"></a>EventProcessorHost
W tym przykładzie użyjemy `EventProcessorHost` dla uproszczenia, jednak może to najlepszy wybór dla tego scenariusza. `EventProcessorHost`nie wykonanej pracy zagwarantować, że nie musisz się martwić o wątków problemy w klasie procesor określonego zdarzenia. W naszym scenariuszu firma Microsoft jest jednak po prostu konwertowania wiadomość do innego formatu i przekazywania go wzdłuż do innej usługi przy użyciu metody asynchroniczne. Istnieje potrzeba aktualizacji stanu współużytkowania i w związku z tym ryzyko wątków problemy. W przypadku większości scenariuszy `EventProcessorHost` prawdopodobnie jest to najlepszy wybór, a na pewno jest opcja łatwiejsza.     

### <a name="ieventprocessor"></a>IEventProcessor
Centralny koncepcja podczas korzystania z `EventProcessorHost` jest utworzenie implementacja `IEventProcessor` interfejs, który zawiera metodę `ProcessEventAsync`. Istota ta metoda jest następujący:

  asynchroniczna {IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) zadania

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

Lista funkcji EventData obiektów są przekazywane do metody i możemy przejść przez tę listę. Bajtów metod pobieranych do obiektu HttpMessage i dany obiekt jest przekazywany do wystąpienia IHttpMessageProcessor.

### <a name="httpmessage"></a>HttpMessage
`HttpMessage` Wystąpienie zawiera trzy danych:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

`HttpMessage` Wystąpienie zawiera `MessageId` identyfikator GUID, który umożliwia łączenie żądania HTTP do odpowiednich odpowiedź HTTP i wartość logiczną, która określa, czy obiekt zawiera wystąpienie HttpRequestMessage i HttpResponseMessage. Przy użyciu wbudowanego przedmiotów HTTP z `System.Net.Http`, I mógł korzystać z `application/http` analizy kod, który znajduje się w `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
`HttpMessage` Wystąpienie jest następnie przesyłany do stosowania `IHttpMessageProcessor` czyli interfejs utworzona rozdzielenie odbieranie i interpretacji terminu z Centrum zdarzeń Azure i rzeczywistego przetwarzania go.


## <a name="forwarding-the-http-message"></a>Przesyłanie dalej wiadomości HTTP
Aby ten przykład I decyzję, że będzie interesujące przekazać żądania HTTP przez do [Runscope](http://www.runscope.com). Runscope to usługa oparte na chmurze, która specjalizuje się w HTTP debugowania, rejestrowania i monitorowania. Mają bezpłatne warstwy, dzięki czemu łatwo wypróbować i pozwala wyświetlić żądania HTTP w czasie rzeczywistym ułożony za pośrednictwem naszej usługi zarządzania interfejsu API.

`IHttpMessageProcessor` Implementacji wygląda tak,

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Mogę mógł korzystać z [istniejących klientów biblioteki Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) , która ułatwia wypychanych `HttpRequestMessage` i `HttpResponseMessage` obiektów w górę do usługi. Aby uzyskać dostęp do interfejsu API Runscope konieczne będzie konto oraz klucz interfejsu API. Instrukcje dotyczące pobierania klucz interfejsu API można znaleźć w screencast [Tworzenie aplikacji programu Access Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) .

## <a name="complete-sample"></a>Całą próbkę
[Kod źródłowy](https://github.com/darrelmiller/ApimEventProcessor) i analiz w próbce znajdują się na Github. Konieczne będzie [Interfejsu API usługi zarządzania](api-management-get-started.md), [Połączonego Centrum zdarzeń](api-management-howto-log-event-hubs.md)i [Konta miejsca do magazynowania](../storage/storage-create-storage-account.md) , aby uruchomić przykład dla siebie.   

Próbka jest tak proste konsoli aplikacją, która wykrywa dla zdarzenia pochodzące z Centrum zdarzeń konwertuje je do `HttpRequestMessage` i `HttpResponseMessage` obiektów, a następnie przekazuje je do interfejsu API Runscope.

Na poniższej ilustracji animowany widać wniosek API w portalu Deweloper aplikacji konsoli przedstawiający wiadomości odebranych, przetwarzania i przekazywane, a następnie żądanie i odpowiedź pojawiać się w ruchu Runscope Inspektor dokumentów.

![Pokaz żądania przesyłane dalej do Runscope](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Podsumowanie
Azure Usługa zarządzania interfejsu API zapewnia doskonale nadaje się do przechwytywania ruchu HTTP do i z usługi interfejsów API. Azure koncentratory zdarzenie jest wysoce skalowalna, ekonomicznych rozwiązanie do przechwytywania ruch i skierowanie go do systemów przetwarzania pomocniczy do logowania, monitorowanie i inne zaawansowane analizy. Nawiązywanie połączenia z 3 ruch firmy monitorowania systemów, takich jak Runscope jest proste jako kilka dozen wiersze kodu.

## <a name="next-steps"></a>Następne kroki
-   Dowiedz się więcej o koncentratory zdarzenia Azure
    -   [Wprowadzenie do koncentratorów zdarzenia Azure](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Odbieranie wiadomości z EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Koncentratory zdarzenia programowania przewodnika](../event-hubs/event-hubs-programming-guide.md)
-   Dowiedz się więcej o integracji interfejsu API zarządzania i koncentratory zdarzenia
    -   [Jak rejestrowanie zdarzeń w Azure koncentratory zdarzenia w zarządzaniu interfejsu API platformy Azure](api-management-howto-log-event-hubs.md)
    -   [Odwołanie do rejestratora jednostki](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [Odwołanie do zasad dziennika do eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    