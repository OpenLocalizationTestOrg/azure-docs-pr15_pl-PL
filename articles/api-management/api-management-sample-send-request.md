<properties
    pageTitle="Za pomocą usługi zarządzania interfejsu API do wygenerowania żądania HTTP"
    description="Dowiedz się, jak nawiązania połączenia usług zewnętrznych z poziomu programu interfejsu API za pomocą zasad i odpowiadania na wezwania w zarządzaniu interfejsu API"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="using-external-services-from-the-azure-api-management-service"></a>Za pomocą usług zewnętrznych z usługi zarządzania interfejsu API platformy Azure

Dostępne w usłudze Azure interfejsu API zarządzania zasadami jak szeroką gamę przydatne pracy wyłącznie na podstawie przychodzące żądanie, odpowiedzi wychodzącej i informacje o konfiguracji podstawowej. Jednak mogą korzystać z usług zewnętrznych z interfejsu API zarządzania otwiera wiele więcej możliwości zasady.

Firma Microsoft wcześniej miały, jak firma Microsoft można wchodzić w interakcje z [usługi Azure Centrum zdarzeń dla rejestrowania, monitorowanie i analizy](api-management-log-to-eventhub-sample.md). W tym artykule możemy wykazać podstawą zasady, które umożliwiają interakcję z dowolnej zewnętrznych HTTP usługi. Te zasady mogą być używane dla powodujące zdalnego zdarzenia lub pobieranie informacji, które będą używane do manipulowania oryginalnego żądania i odpowiedzi w określony sposób.

## <a name="send-one-way-request"></a>Sposób żądanie, wysłania z nich w-
Prawdopodobnie najłatwiejszym interakcja zewnętrznych jest styl fire i zapomnisz żądanie, które umożliwia zewnętrznej usługi Cię powiadamiać o niektórych rodzajów ważne wydarzenia. Firma Microsoft korzysta z zasad przepływu sterowania `choose` wykrywanie dowolny warunek, że firma Microsoft interesuje, a następnie, jeśli warunek jest spełniony, możemy wprowadź zewnętrznych żądania HTTP za pomocą zasad [sposób żądanie, wysłania z nich w-](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) . Może to być żądanie systemu obsługi wiadomości, takie jak Hipchat lub zapas czasu lub Poczta interfejsu API takie jak SendGrid lub MailChimp lub dla zdarzenia najważniejsze na przykład PagerDuty. Wszystkie te systemy obsługi wiadomości mają proste interfejsów API HTTP, które można było łatwo wywołania.

### <a name="alerting-with-slack"></a>Alerty z zapas czasu
Poniższy przykład pokazuje, jak wysyłać wiadomości do pokoju rozmów zapasu czasu, jeśli kod stanu odpowiedzi protokołu HTTP jest większe niż lub równe 500. Błąd 500 zakresu wskazuje problem z naszych wewnętrzną interfejsu API klienta naszą interfejsu API nie może rozpoznać się. Zazwyczaj wymaga niektórych rodzajów interwencji na naszych części.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Zapas czasu zawiera pojęcia haki przychodzących sieci web. Konfigurując hak przychodzących sieci web, zapas czasu generuje specjalnego adresu URL, dzięki czemu można żądanie POST prosty i do przekazania wiadomości do kanału zapasu czasu. Treść JSON, który tworzymy jest oparty na format zdefiniowany przez zapas czasu.

![Hak zapasu czasu w sieci Web](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Jest fire i zapomnij wystarczająco dobry?
Istnieją pewne kompromisów po za pomocą stylu fire i zapomnisz żądania. Jeśli jakiegoś powodu żądanie kończy się niepowodzeniem, a następnie nie jest informowany awarii. W tej sytuacji określonego złożoność o awarii pomocniczej raportowania systemu i koszty dodatkowe Trwa oczekiwanie na odpowiedź nie jest uzasadnione. W przypadku scenariuszy tam, gdzie jest sprawdzenie odpowiedź następnie [Wyślij wezwanie](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) jest lepszym rozwiązaniem.

## <a name="send-request"></a>Żądanie wysłania
`send-request` Umożliwia zasady za pomocą zewnętrznej usługi do wykonywania zadań złożonych obliczeń i zwrócić danych w celu zarządzania interfejsu API usługi, który może być używany do dalszego przetwarzania zasad.

### <a name="authorizing-reference-tokens"></a>Wydające tokenów odwołań
Głównych funkcji zarządzania interfejsu API jest ochrona zasobów wewnętrznej bazy danych. Jeśli serwer autoryzacji używany przez usługi interfejsu API tworzy [JWT tokeny](http://jwt.io/) jako część przepływ uwierzytelnianie OAuth2, jak [Usługi Azure Active Directory](../active-directory/active-directory-aadconnect.md) , a następnie można użyć `validate-jwt` zasad do sprawdzania poprawności tokenu. Jednak niektóre serwery autoryzacji utworzyć, co to są nazywane [tokenów odwołań](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , którego nie można zweryfikować bez nawiązywania połączenia na serwerze autoryzacji.

### <a name="standardized-introspection"></a>Standardowe introspection
W przeszłości została żadnym standardowym weryfikacji token odwołania z serwerem autoryzacji. Jednak ostatnio proponowane standardowy [RFC 7662](https://tools.ietf.org/html/rfc7662) został opublikowany przez IETF definiujące, jak serwer zasobów można sprawdzić poprawność token.

### <a name="extracting-the-token"></a>Wyodrębnianie tokenu
Pierwszym krokiem jest używane do wyodrębniania tokenu w nagłówku autoryzacji. Wartość nagłówka należy sformatowanym z użyciem `Bearer` schematu autoryzacji, pojedynczą spację, a następnie token autoryzacji zgodnie [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Niestety istnieją przypadki, w której zostanie pominięty schematu autoryzacji. Aby to uwzględniać podczas analizy, możemy dzielenie wartość nagłówka w obszarze i wybierz ostatni ciąg z zwróconej tablicy ciągów. Zapewnia to obejście źle sformatowana autoryzacji nagłówków.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>Tworzenie żądania sprawdzenia poprawności
Gdy mamy już token autoryzacji, firma Microsoft może mieć żądanie tak, aby sprawdzić poprawność tokenu. RFC 7662 połączeń introspection ten proces i wymaga, aby użytkownik `POST` formularza HTML do zasobu introspection. Formularz HTML przy użyciu klucza musi zawierać co najmniej pary klucz wartość `token`. To żądanie na serwerze autoryzacji również muszą zostać uwierzytelnione aby upewnić się, że złośliwy klientów nie Przejdź połowów włokiem tokenów prawidłowe.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Sprawdzanie odpowiedzi
`response-variable-name` Atrybut jest używany do udostępnienia zwracane odpowiedzi. Nazwy zdefiniowane w tej właściwości mogą być używane jako klucz do `context.Variables` słownika, aby uzyskać dostęp do `IResponse` obiektu.

Z obiektu odpowiedzi firma Microsoft można pobrać treści i RFC 7622 nam informuje, że odpowiedź musi być obiektem JSON i musi zawierać co najmniej właściwość o nazwie `active` to jest wartością logiczną. Gdy `active` jest PRAWDA, a następnie tokenu zachowuje ważność.

### <a name="reporting-failure"></a>Błąd raportowania
Firma Microsoft korzysta z `<choose>` zasad wykrywanie Jeśli token jest nieprawidłowy i zwraca 401 odpowiedź.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Zgodnie z [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) w tym artykule opisano sposób `bearer` tokeny należy również zwróconych `WWW-Authenticate` nagłówka z 401 odpowiedź. Uwierzytelnienia WWW jest przeznaczona do poinstruuj klienta dotyczące tworzenia prawidłowo autoryzowanych wezwanie. Ze względu na wielu różnych metod niemożliwe w ramach uwierzytelnianie OAuth2 trudno komunikowanie się wszystkie potrzebne informacje. Na szczęście istnieje działań, na przykład do pomocy [klientów Dowiedz się, jak prawidłowo autoryzacji żądania na serwerze zasobów](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Rozwiązanie wersji ostatecznej
Zestawienie wszystkich części, będziemy korzystać z następujących zasad:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

Jest to tylko jeden z wielu przykłady jak `send-request` zasad można zintegrować proces zaproszenia i odpowiedzi ułożony za pośrednictwem usługi zarządzania interfejsu API usługi zewnętrzne.

## <a name="response-composition"></a>Skład odpowiedzi
`send-request` Zasady mogą być używane dotyczące zwiększania żądania podstawowego do systemu wewnętrznej bazy danych, jak firma Microsoft pokazano w poprzednim przykładzie lub może służyć jako ukończone Zamień dla połączenia wewnętrznej bazy danych. Za pomocą tej techniki możemy łatwo utworzyć złożone zasoby, które są połączone z wielu różnych systemów.

### <a name="building-a-dashboard"></a>Tworzenie pulpitu nawigacyjnego   
Czasami chcesz być w stanie udostępniania informacji, która istnieje w wiele systemów wewnętrznej bazy danych, na przykład, aby sterują pulpitu nawigacyjnego. Kluczowych wskaźników wydajności pochodzić z wszystkich różnych wstecz końców, ale nie chcesz zapewnić bezpośredni dostęp do nich i będzie i jeśli można pobrać wszystkich informacji z jednego żądania. Być może niektóre informacje wewnętrznej bazy danych musi niektórych odcięć i kostkowania i nieco dezynfekcja najpierw! Możliwość buforowania tego zasobu złożone będą przydatne w celu zmniejszenia obciążenia wewnętrznej bazy danych, jak wiadomo, że użytkownicy mają rodzaj kucie klawisz F5, aby sprawdzić, czy ich underperforming metryki może się zmienić.    

### <a name="faking-the-resource"></a>Faking zasobu
Pierwszy krok do tworzenia naszych zasobów pulpitu nawigacyjnego jest konfigurowanie nowej operacji w portalu zarządzania interfejsu API programu publisher. Są to operacji symbol zastępczy umożliwia konfigurowanie nasze zasady skład do utworzenia naszych zasobu dynamicznego.

![Operacja pulpitu nawigacyjnego](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Tworzenie żądania
Po `dashboard` operacji została utworzona możemy Konfigurowanie zasad specjalnie dla tej operacji. 

![Operacja pulpitu nawigacyjnego](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Pierwszym krokiem jest wyodrębnić parametry kwerendy z przychodzące żądanie tak, aby było ich przekazywania do naszych wewnętrznej bazy danych. W tym przykładzie naszych pulpitu nawigacyjnego jest widoczna informacje oparte na określonym czasie dlatego `fromDate` i `toDate` parametru. Czy firma Microsoft korzysta z `set-variable` zasad, aby wyodrębnić dane z adresu URL żądania.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Gdy mamy tych informacji firma Microsoft można wprowadzać żądania do systemów wewnętrznej bazy danych. Każdego żądania tworzy nowy adres URL z parametrów połączenia jego odpowiednich server i przechowuje odpowiedzi w zmiennej kontekstu.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Te żądania będzie wykonywać w kolejności, która nie jest idealnym rozwiązaniem. W nowej wersji możemy być wprowadzenie nowych zasad o nazwie `wait` umożliwi wszystkie te żądania wykonania równolegle.

### <a name="responding"></a>Odpowiadanie

Do utworzenia złożonych odpowiedzi firma Microsoft korzysta z zasad [zwrotu odpowiedź](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) . `set-body` Element można użyć wyrażenia do utworzenia nowego `JObject` z reprezentacji składnik osadzony jako właściwości.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Pełne zasady wygląda następująco:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

W konfiguracji symbolu zastępczego operacji możemy Konfigurowanie zasobów pulpitu nawigacyjnego do buforowanej co najmniej przez godzinę, ponieważ firma Microsoft wie, charakter danych oznacza, że nawet w przypadku godziny nieaktualny, nadal będzie wystarczająco skuteczne przekazuj ważnych informacji do użytkowników.

## <a name="summary"></a>Podsumowanie
Azure usługi zarządzania interfejsu API oferuje elastyczne zasady, które mogą być stosowane selektywne ruchu HTTP i umożliwia skład usług wewnętrznej bazy danych. Czy chcesz zwiększyć Centrum interfejsu API z alertu weryfikacji funkcje, funkcje sprawdzania poprawności lub tworzenia nowych złożone zasobów według wielu usług wewnętrznej bazy danych, `send-request` i związane z nią zasady Otwórz świata możliwości.

## <a name="watch-a-video-overview-of-these-policies"></a>Obejrzyj klip wideo pokazujący tych zasad
Aby uzyskać więcej informacji na [sposób żądanie, wysłania z nich w-](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [Wyślij żądanie](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)i zasady [odpowiedź zwracana](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) omówione w tym artykule, należy w poniższym klipie wideo.

> [AZURE.VIDEO send-request-and-return-response-policies]
