<properties 
    pageTitle="Wyzwalacze aplikacji usługi aplikacji interfejsu API | Microsoft Azure" 
    description="Jak zaimplementować wyzwalaczy w aplikacji interfejsu API usługi aplikacji Azure" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure interfejs API usługi aplikacji aplikacji wyzwalaczy

>[AZURE.NOTE] Tej wersji artykuł dotyczy wersji schematu 2014-12-01-podgląd aplikacji interfejsu API.


## <a name="overview"></a>Omówienie

W tym artykule wyjaśniono, jak zaimplementować interfejsu API aplikacji wyzwalaczami i używanie ich za pomocą aplikacji logicznych.

Wszystkie fragmenty kodu, w tym temacie są kopiowane z [Przykładowy kod aplikacji interfejsu API FileWatcher](http://go.microsoft.com/fwlink/?LinkId=534802). 

Należy zauważyć, że musisz pobrać pakiet nuget następujący kod w tym artykule Tworzenie i uruchamianie: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Co to są wyzwalaczami aplikacji interfejsu API?

Jest typowy scenariusz dla aplikacji interfejsu API uruchomienie zdarzenia, tak aby klientów aplikacji interfejsu API można wykonać odpowiednie kroki w odpowiedzi na zdarzenie. Mechanizmu interfejsu API usługi REST podstawie obsługującego w tym scenariuszu nosi nazwę wyzwalacza aplikacji interfejsu API. 

Na przykład załóżmy, że kod klienta korzysta z [serwisu Twitter interfejsu API łącznik aplikacji](../app-service-logic/app-service-logic-connector-twitter.md) i kodzie musi wykonać czynności w oparciu o nowych tweety, które zawierają określone wyrazy. W tym przypadku można skonfigurować wyzwalacz ankietę lub wypychanych ułatwiający tę potrzebę.

## <a name="poll-trigger-versus-push-trigger"></a>Ankiety wyzwalacza i wyzwalacz wypychania

Obecnie obsługiwane są dwa typy wyzwalaczy:

- Ankiety wyzwalacza - klienta sprawdza aplikacji interfejsu API dla powiadomienia o zostały uruchamiany zdarzenia 
- Wyzwalacz wypychania - klienta jest powiadamiany za pomocą aplikacji interfejsu API po zdarzenia 

### <a name="poll-trigger"></a>Wyzwalacza ankiety

Wyzwalacz ankiety jest zaimplementowana jako zwykłego interfejsu API usługi REST i zamierza ankieta go, aby można było uzyskiwać powiadomienie o swoich klientów (na przykład aplikacja logiczny). Gdy klient może zachować stan, wyzwalacz ankiety, sam jest stateless. 

Następujące informacje dotyczące pakietów i odpowiadania na wezwania przedstawiono kilka kluczowych aspektów umowy wyzwalacza ankiety:

- Żądanie
    - Metoda HTTP: pobieranie
    - Parametry
        - triggerState - parametr opcjonalny umożliwia klientów określić, że ich stanu, aby wyzwalacza ankiety prawidłowo można zdecydować, czy zwraca powiadomienie lub nie na podstawie określonej stanu.
        - Parametry specyficzne dla interfejsu API
- Odpowiedź
    - Kod stanu **200** - żądanie jest prawidłowe, a powiadomienia wyzwalacza. Zawartość powiadomienie będzie treść odpowiedzi. Nagłówek "Ponów próbę po" w odpowiedzi wskazuje, że dane dodatkowe powiadomienia muszą być pobierane z połączeniem następne żądanie.
    - Kod stanu **202** - żądanie jest prawidłowe, ale jest dostępne żadne nowe powiadomienie z wyzwalacza.
    - Kodu stanu **4xx** — żądanie nie jest prawidłowe. Klient nie powinien ponów żądanie.
    - Kod stanu **5xx** — żądanie spowodowało wewnętrzny błąd serwera i/lub tymczasowy problem. Klient powinien ponawianie żądania.

Poniższy fragment kodu jest przykładem jak wdrażać wyzwalacza ankiety.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Aby przetestować ten wyzwalacz ankiety, wykonaj następujące czynności:

1. Wdrażanie aplikacji interfejsu API z ustawieniem uwierzytelnianie **anonimowe publicznej**.
2. Zadzwoń **dotknij** operacji dostępu do pliku. Poniższa ilustracja przedstawia przykładowe żądanie za pośrednictwem Postman.
   ![Operacja dotyku połączenia za pośrednictwem Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Wywołuje wyzwalacza ankiety z parametrem **triggerState** ustawionym sygnaturę czasową przed kroku nr 2. Poniższa ilustracja przedstawia przykładowe żądanie za pośrednictwem Postman.
   ![Wyzwalacza ankiety połączenia za pośrednictwem Postman](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Wyzwalacz wypychania

Wyzwalacz wypychania jest zaimplementowana jako zwykłego interfejsu API usługi REST, które umieszcza powiadomienia dla klientów, którzy zarejestrowanych powiadamianie o dostępności określonego zdarzenia.

Następujące informacje dotyczące pakietów i odpowiadania na wezwania przedstawiają niektóre aspektami umowy wyzwalacza wypychania.

- Żądanie
    - Metoda HTTP: umieszczanie
    - Parametry
        - Identyfikator_wyzwalacza: wymagane — nieprzezroczysty ciągu (na przykład GUID) reprezentujący rejestracji wyzwalacz wypychania.
        - callbackUrl: wymagane — adres URL zwrotnego wywołania po wydarzenia. Wywoływanie jest proste połączenia HTTP wpisu.
        - Interfejs API określonych parametrów
- Odpowiedź
    - Stan kod **200** - wniosek o zarejestrowanie klienta pomyślnie.
    - Kodu stanu **4xx** — żądanie nie jest prawidłowe. Klient nie powinien ponów żądanie.
    - Kod stanu **5xx** — żądanie spowodowało wewnętrzny błąd serwera i/lub tymczasowy problem. Klient powinien ponów żądanie.
- Zwrotnego
    - Metoda HTTP: WPIS
    - Treści żądania: powiadomienie o zawartości.

Przykładem jak wdrażać wyzwalacz wypychania jest następujący fragment kodu:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Aby przetestować ten wyzwalacz ankiety, wykonaj następujące czynności:

1. Wdrażanie aplikacji interfejsu API z ustawieniem uwierzytelnianie **anonimowe publicznej**.
2. Przejdź do [http://requestb.in/](http://requestb.in/) tworzenie RequestBin, która będzie służyć jako adres URL zwrotnego.
3. Zadzwoń wyzwalacz z GUID jako **Identyfikator_wyzwalacza** i adres URL RequestBin jako **callbackUrl**.
   ![Wyzwalacz wypychania połączenia za pośrednictwem Postman](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Zadzwoń **dotyku** operacji dostępu do pliku. Poniższa ilustracja przedstawia przykładowe żądanie za pośrednictwem Postman.
   ![Operacja dotyku połączenia za pośrednictwem Postman](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Sprawdzanie RequestBin, aby potwierdzić, że zwrotnego wyzwalacza wypychania jest wywoływana z właściwości dane wyjściowe.
   ![Wyzwalacza ankiety połączenia za pośrednictwem Postman](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Opisz wyzwalaczy w definicji interfejsu API

Po implementacji wyzwalacze i wdrażanie aplikacji interfejsu API Azure, przejdź do karta **Definicji interfejsu API** w portal Azure preview i pojawi się, że wyzwalacze są automatycznie uznawane w interfejsie użytkownika, wynika Swagger interfejsu API 2.0 definicji aplikacji interfejsu API.

![Karta definicji interfejsu API](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Jeśli przycisk **Pobierz Swagger** i Otwórz plik, JSON, zobaczysz wyniki podobne do następujących czynności:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

Właściwość rozszerzenia **x-ms-schedular wyzwalacza** jest jak wyzwalaczy opisano w definicji interfejsu API i są automatycznie dodawane przez bramę aplikacji interfejsu API, gdy użytkownik zażąda definicję interfejsu API za pomocą bramy, jeśli wezwanie na jeden z poniższych kryteriów. (Możesz również dodać tę właściwość ręcznie.)

- Wyzwalacza ankiety
    - Jeśli **Metoda HTTP jest.**
    - Jeśli właściwość **operationId** zawiera ciąg **wyzwalacza**.
    - Jeśli właściwość **parameters** zawiera parametr właściwość **nazwy** , jest ustawiona na **triggerState**.
- Wyzwalacz wypychania
    - Jeśli metoda HTTP jest **umieszczenie**.
    - Jeśli właściwość **operationId** zawiera ciąg **wyzwalacza**.
    - Jeśli właściwość **parameters** zawiera parametr właściwość **nazwy** , jest ustawiona na **Identyfikator_wyzwalacza**.

## <a name="use-api-app-triggers-in-logic-apps"></a>W aplikacjach logiczny za pomocą interfejsu API aplikacji wyzwalaczy

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Wyświetlanie i konfigurowanie wyzwalaczy aplikacji interfejsu API w Projektancie aplikacji warunków logicznych

Po utworzeniu aplikacji logika w tej samej grupy zasobów jako aplikacja interfejsu API można dodać ją do obszaru roboczego projektanta po prostu, klikając go. Poniższe obrazy pokazują to:

![Wyzwalacze w Projektancie aplikacji warunków logicznych](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Konfigurowanie wyzwalacza ankiety w Projektancie aplikacji warunków logicznych](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Konfigurowanie wyzwalacza wypychanych w Projektancie aplikacji warunków logicznych](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Optymalizowanie wyzwalacze aplikacji interfejsu API dla aplikacji logicznych

Po dodaniu wyzwalaczy do aplikacji interfejsu API istnieje kilka rzeczy, które można wykonywać w celu zwiększenia możliwości podczas korzystania z aplikacji interfejsu API w aplikacji logicznych.

Na przykład parametr **triggerState** wyzwalaczy ankiety powinna być równa następujące wyrażenie w aplikacji logicznych. To wyrażenie należy ocenianie ostatniej wywołanie wyzwalacza z aplikacji logiki i tę wartość zwracana.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Uwaga: Aby objaśnienia funkcje używane w powyższe wyrażenie, zapoznaj się z dokumentacją na [Język definicji przepływu pracy aplikacji logika](https://msdn.microsoft.com/library/azure/dn948512.aspx).

Użytkownicy aplikacji logika powinny zawierać wyrażenie powyżej parametru **triggerState** podczas korzystania z wyzwalacza. Użytkownik może mieć wartość wstępnie ustawione przez projektanta aplikacji logika za pośrednictwem właściwości rozszerzenia **x-ms harmonogram zalecenie**.  Właściwość rozszerzenia **x-ms widoczność** można ustawić na wartość *wewnętrznej* , tak, aby parametr się nie jest wyświetlana na projektanta.  Poniższy fragment pokazuje, które.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Wyzwalacze wypychanych parametru **Identyfikator_wyzwalacza** musi jednoznacznie identyfikować aplikacji logicznych. Zalecane dobrym rozwiązaniem jest ustawiona na nazwę przepływu pracy przy użyciu następującego wyrażenia:

    @workflow().name

Za pomocą właściwości rozszerzenia **x-ms harmonogram zalecenie** i **x-ms widoczność** w jego definicji interfejsu API aplikacji interfejsu API można przekazać do projektanta aplikacji logika ustawione automatycznie tego wyrażenia dla użytkownika.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Dodawanie właściwości rozszerzenia w definicji interfejsu API

Informacje dodatkowe metadane — takie jak właściwości rozszerzenia **x-ms harmonogram zalecenie** i **x-ms widoczność** — można dodawać w definicji interfejsu API w jeden z dwóch sposobów: statyczną lub dynamiczną.

Statyczne metadane można bezpośrednio edytować plik */metadata/apiDefinition.swagger.json* w projekcie i ręczne dodawanie właściwości.

W przypadku aplikacji interfejsu API przy użyciu metadanych dynamiczne można edytować plik SwaggerConfig.cs, aby dodać filtr operacji, które można dodać te rozszerzenia.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Oto przykładowy sposób tej klasy można zastosować do ułatwienia scenariusz dynamiczne metadanych.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
