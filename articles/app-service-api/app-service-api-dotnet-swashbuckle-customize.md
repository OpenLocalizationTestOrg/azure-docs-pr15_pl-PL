
<properties 
    pageTitle="Dostosowywanie definicje wygenerowane Swashbuckle interfejsu API" 
    description="Dowiedz się, jak dostosować definicje Swagger interfejsu API, wygenerowane przez Swashbuckle aplikacji interfejsu API usługi aplikacji Azure." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>Dostosowywanie definicje wygenerowane Swashbuckle interfejsu API 

## <a name="overview"></a>Omówienie

W tym artykule wyjaśniono, jak dostosować Swashbuckle do obsługi typowych scenariuszy, gdzie możesz zechcieć zmienić domyślne zachowanie:

* Operacja zduplikowane identyfikatory przeciążenia metody kontroler generuje Swashbuckle
* Swashbuckle założono, że tylko prawidłowej odpowiedzi przy użyciu metody jest 200 HTTP (OK) 
 
## <a name="customize-operation-identifier-generation"></a>Dostosowywanie generowania identyfikator operacji

Swashbuckle tworzy Swagger identyfikatory operacji łączenia nazwę kontrolera i nazwa metody. Ten wzorzec tworzy problem, jeśli masz wiele przeciążenia metody: Swashbuckle generuje identyfikatory zduplikowanych operacji, która jest nieprawidłowy Swagger JSON.

Na przykład poniższy kod kontroler powoduje Swashbuckle wygenerować trzy identyfikatory operacji Contact_Get.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Możesz ręcznie rozwiązać problem, nadając metody unikatowych nazw, takich jak poniższe czynności w tym przykładzie:

* Pobierz
* GetById
* GetPage

Alternatywny ma rozszerzenie Swashbuckle umożliwia automatyczne generowanie operacji unikatowych identyfikatorów.

Poniższe kroki pokazują jak dostosować Swashbuckle za pomocą pliku *SwaggerConfig.cs* , który znajduje się w projekcie w szablonie projekt Visual Studio interfejsu API Apps Preview.  Można także dostosować Swashbuckle w projekcie interfejs API sieci Web, które Konfigurowanie rozmieszczania jako aplikacji interfejsu API.

1. Tworzenie niestandardowego `IOperationFilter` implementacji 

    `IOperationFilter` Interfejs zapewnia punkt rozszerzeń dla użytkowników Swashbuckle, aby dostosować różnych aspektów procesu metadanych Swagger. Poniższy kod ilustruje jednym ze sposobów zmieniania zachowanie operacji identyfikator generacji. Kod dołącza nazwy parametrów na nazwę identyfikatora operacji.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. W pliku *App_Start\SwaggerConfig.cs* , zadzwoń pod numer `OperationFilter` metody spowodowało Swashbuckle korzystać z nowych `IOperationFilter` implementacji.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    Plik *SwaggerConfig.cs* , który zostanie usunięty przez pakiet Swashbuckle NuGet zawiera wiele przykładów ujęta w nowym punktów rozszerzania. Dodać kolejne komentarze nie są wyświetlane w tym miejscu. 

    Po wprowadzeniu tej zmiany do `IOperationFilter` wykonania jest używana i powoduje, że operacja unikatowych identyfikatorów do wygenerowania.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Zezwalaj na kody odpowiedzi innych niż 200

Domyślnie Swashbuckle zakłada, że odpowiedzi HTTP 200 (OK) jest *tylko* wiarygodnych odpowiedzi przy użyciu metody interfejsu API sieci Web. W niektórych przypadkach warto zwracać inne kody odpowiedzi nie powodując klienta podnieść wyjątek.  Na przykład poniższy kod interfejs API sieci Web pokazano scenariusza, w którym chcieć klienta, aby zaakceptować 200 lub 404 jako prawidłowych odpowiedzi.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

W tym scenariuszu Swagger, które domyślnie generuje Swashbuckle określa tylko jeden wiarygodnych kod stanu HTTP, HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Ponieważ Visual Studio używa definicji Swagger interfejsu API w celu wygenerowania kodu dla klienta, tworzy kod klienta, który wywołuje wyjątku dla odpowiedzi innych niż 200 HTTP. Kod poniżej jest w kliencie C# wygenerowane dla tej metody interfejsu API usług Web próbki.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle udostępnia dwa sposoby dostosowywania listy oczekiwanych kodów odpowiedzi HTTP, które generuje przy użyciu komentarzy XML lub `SwaggerResponse` atrybut. Atrybut jest łatwiejsze, ale tylko jest dostępna w Swashbuckle 5.1.5 lub nowszej. Szablon aplikacji interfejsu API Podgląd nowego projektu programu Visual Studio 2013 zawiera Swashbuckle wersji 5.0.0, więc jeśli używany szablon i nie chcesz, aby zaktualizować Swashbuckle, jedyną możliwością jest używać komentarzy XML. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Dostosowywanie kodów oczekiwano odpowiedzi przy użyciu komentarzy XML

Ta metoda umożliwia określenie kody odpowiedzi, w przypadku wersji Swashbuckle wcześniej niż 5.1.5.

1. Najpierw dodaj komentarzy dokumentacji XML nad metodami, które chcesz określić kody odpowiedzi HTTP. Pobieranie próbki interfejs API sieci Web akcji, jak pokazano powyżej i stosowania dokumentacji XML do niego doprowadziłaby kodu, jak w następującym przykładzie. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Dodawanie instrukcji w pliku *SwaggerConfig.cs* do kierowania Swashbuckle do użycia XML pliku dokumentacji.

    * Otwórz *SwaggerConfig.cs* i utworzyć metodę w klasie *SwaggerConfig* , aby określić ścieżkę do pliku XML dokumentacji. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Przewiń w dół w pliku *SwaggerConfig.cs* do momentu wyświetlenia linii ujęta w nowym kodu podobne zrzut poniżej ekranu. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Usuń komentarze wiersz, aby włączyć komentarze XML przetwarzania podczas generowania Swagger. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Aby wygenerować plik dokumentacji XML, przejdź do właściwości projektu i Włącz plik dokumentacji XML, jak pokazano poniżej ekranu. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Po wykonaniu tych kroków Swagger JSON, wygenerowane przez Swashbuckle zostaną zastosowane kody odpowiedzi HTTP określonych w komentarzach XML. Zrzut ekranu poniżej przedstawia ten nowy ładunku JSON. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Gdy Generuj kod klienta dla interfejsu API usługi REST za pomocą programu Visual Studio, kod C# akceptuje HTTP OK i nie można odnaleźć kody stanu bez podniesienie wyjątek, umożliwiając dużo kodu do podejmowania decyzji o sposobie obsługi zwrotu null rekord kontaktu. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

Kod ta można znaleźć w [tym repozytorium GitHub](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Wraz z interfejsu API sieci Web projekt oznaczonych komentarzy dokumentacji XML jest projekt aplikacji konsoli, który zawiera wygenerowane klienta dla tego interfejsu API. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Dostosowywanie przy użyciu atrybutu SwaggerResponse kody oczekiwano odpowiedzi

Atrybut [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) jest dostępna w Swashbuckle 5.1.5 lub nowszy. W przypadku, gdy masz wcześniejszą wersję w projekcie, ta sekcja rozpoczyna się od wyjaśnieniem, jak zaktualizować pakiet Swashbuckle NuGet, tak aby można było używać tego atrybutu.

1. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy projektu interfejs API sieci Web, a następnie kliknij pozycję **Zarządzaj pakietów NuGet**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. Kliknij przycisk *Aktualizuj* obok pakietu NuGet *Swashbuckle* . 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. Dodawanie atrybutów *SwaggerResponse* metod akcji interfejs API sieci Web, dla których chcesz określić prawidłowe kody odpowiedzi HTTP. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Dodawanie `using` poufności informacji dla atrybutu nazw:

        using Swashbuckle.Swagger.Annotations;
        
1. Przejdź do adresu URL */swagger/docs/v1* projektu i różnych kodów odpowiedzi HTTP będą widoczne w Swagger JSON. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Kod ta można znaleźć w [tym repozytorium GitHub](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Wraz z interfejsu API sieci Web projekt udekorowanego z atrybutem *SwaggerResponse* jest projekt aplikacji konsoli, który zawiera wygenerowane klient dla tego interfejsu API. 

## <a name="next-steps"></a>Następne kroki

Ten artykuł pokazuje, jak dostosować sposób Swashbuckle generuje identyfikatory operacji i kody prawidłowa odpowiedź. Aby uzyskać więcej informacji zobacz [Swashbuckle na GitHub](https://github.com/domaindrivendev/Swashbuckle).
 
