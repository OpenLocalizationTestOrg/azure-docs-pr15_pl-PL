<properties
    pageTitle="W aplikacji usługi pomocy technicznej CORS | Microsoft Azure"
    description="Dowiedz się, jak użyć funkcji CORS w Azure Azure aplikacji usługi."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/27/2016"
    ms.author="rachelap"/>

# <a name="consume-an-api-app-from-javascript-using-cors"></a>Używanie aplikacji interfejsu API z kodu JavaScript za pomocą CORS

Usługa aplikacji oferuje Obsługa [Krzyżowe Origin zasobów udostępniania (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), który umożliwia klientom nawiązywania połączeń innej domeny do interfejsów API usługi, które są obsługiwane w aplikacjach interfejsu API języka JavaScript. Aplikacji usługi umożliwia konfigurowanie CORS dostępu do swojego interfejsu API bez pisania kodów z interfejsu API.

Ten artykuł zawiera dwie sekcje:

* W sekcji [sposobu konfigurowania CORS](#corsconfig) ogólnie wyjaśniono sposób konfigurowania CORS dla dowolnej aplikacji interfejsu API, web app lub aplikacji dla urządzeń przenośnych. Dotyczy to jednakowo wszystkie struktury, które są obsługiwane przez usługi aplikacji, w tym .NET, Node.js i Java. 

* Począwszy od [kontynuowaniem samouczki .NET wprowadzenie do](#tutorialstart) sekcji tego artykułu jest demonstrujący obsługi CORS w oparciu o Tobie w [pierwszej aplikacji interfejsu API wprowadzenie Uruchamianie samouczka](app-service-api-dotnet-get-started.md)samouczek. 

## <a id="corsconfig"></a>Jak skonfigurować CORS w Azure aplikacji usługi

Możesz skonfigurować CORS Azure portal lub za pomocą narzędzia [Azure Menedżera zasobów](../azure-resource-manager/resource-group-overview.md) .

#### <a name="configure-cors-in-the-azure-portal"></a>Konfigurowanie CORS w portalu Azure

8. W przeglądarce przejdź do [portalu Azure](https://portal.azure.com/).

2. Kliknij pozycję **Usługi aplikacji**, a następnie kliknij nazwę aplikacji interfejsu API.

    ![Wybierz aplikację interfejsu API w portalu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. W kartę **Ustawienia** , która zostanie otwarta po prawej stronie karta **interfejsu API aplikacji** Znajdź sekcję **interfejsu API** , a następnie kliknij **CORS**.

    ![Wybierz pozycję CORS w karta Ustawienia](./media/app-service-api-cors-consume-javascript/clicksettings.png)

11. W polu Tekst wprowadź adres URL lub adresy URL, które chcesz zezwalać na rozmowy JavaScript pochodzą.


    Na przykład jeśli zainstalowano aplikację JavaScript do aplikacji sieci web o nazwie todolistangular, wprowadź "https://todolistangular.azurewebsites.net". Alternatywnie możesz wprowadzić gwiazdka (*), aby określić, że wszystkie domeny pochodzenia są akceptowane.


13. Kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Po kliknięciu przycisku **Zapisz**aplikacji interfejsu API akceptuje JavaScript połączeń z określonych adresów URL.

#### <a name="configure-cors-by-using-azure-resource-manager-tools"></a>Konfigurowanie CORS przy użyciu narzędzia Menedżera zasobów Azure

Można również skonfigurować CORS aplikacji interfejsu API przy użyciu [szablonów Menedżera zasobów Azure](../resource-group-authoring-templates.md) w wierszu polecenia narzędzi, takich jak [Azure programu PowerShell](../powershell-install-configure.md) i [Polecenie Azure](../xplat-cli-install.md). 

Na przykład szablon Menedżera zasobów Azure, który ustawia właściwości CORS Otwórz [plik azuredeploy.json w repozytorium tego samouczka przykładowej aplikacji](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Znajdź sekcję szablonu, który wygląda następująco:

        "cors": {
            "allowedOrigins": [
                "todolistangular.azurewebsites.net"
            ]
        }

## <a id="tutorialstart"></a>Kontynuowanie samouczek Wprowadzenie do programu .NET

Jeśli używasz Node.js lub Java — wprowadzenie serii w przypadku aplikacji interfejsu API, ukończono uzyskiwania serii wprowadzenie. Przejdź do sekcji [Następne kroki](#next-steps) , aby znaleźć sugestie dotyczące dalsze informacje na temat aplikacji interfejsu API.

W dalszej części tego artykułu jest kontynuacji serii — wprowadzenie .NET i zakłada się pomyślnie ukończona [pierwszą samouczka](app-service-api-dotnet-get-started.md).

## <a name="deploy-the-todolistangular-project-to-a-new-web-app"></a>Wdrażanie projektu ToDoListAngular nowej aplikacji sieci web

W [pierwszym samouczek](app-service-api-dotnet-get-started.md)utworzono aplikacji interfejsu API warstwa środkowa i aplikacji interfejsu API warstwa dla danych. W tym samouczku tworzenie aplikacji sieci web dla aplikacji jednostronicowy (SPA), aplikacja połączeń interfejsu API inicjał drugiego poziomu. Dla funkcji bezpiecznego uwierzytelniania HASŁA można pracować musisz włączyć aplikację warstwa środkowa interfejsu API CORS. 

W [aplikacji przykładowej listy zadań](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list)projektu ToDoListAngular jest proste klienta AngularJS, który wymaga projektu interfejs API sieci Web ToDoListAPI warstwy środkowej. Kod JavaScript w pliku *app/scripts/todoListSvc.js* połączeń API przy użyciu protokołu HTTP AngularJS dostawcy. 

        angular.module('todoApp')
        .factory('todoListSvc', ['$http', function ($http) {

            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
        
            return {
                getItems : function(){
                    return $http.get(apiEndpoint + '/api/TodoList');
                },

                /* Get by ID, Put, and Delete methods not shown */

                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/TodoList', item);
                }
            };
        }]);

### <a name="create-a-new-web-app-for-the-todolistangular-project"></a>Tworzenie nowej aplikacji sieci web dla projektu ToDoListAngular

Procedura tworzenia nowej aplikacji sieci web aplikacji usługi i wdrażanie projektu jest podobna do pokazano służące do [tworzenia i wdrażanie aplikacji interfejsu API w pierwszej instrukcji w tej serii](app-service-api-dotnet-get-started.md#createapiapp). Jedyną różnicą jest typem aplikacji **Web App** zamiast **Interfejsu API aplikacji**.  Aby uzyskać zrzutów ekranu z okien dialogowych zobacz 

1. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy ToDoListAngular projektu, a następnie kliknij **Publikuj**.

3.  Na karcie **profil** kreatora **Publikowania w sieci Web** kliknij pozycję **Microsoft Azure aplikacji usługi**.

5. W oknie dialogowym **Aplikacji usługi** kliknij przycisk **Nowy**.

3. Na karcie **macierzyste** okna dialogowego **Tworzenie aplikacji usługi** wprowadź **Nazwę aplikacji sieci Web** , która jest unikatowa w domenie *azurewebsites.net* . 

5. Wybierz Azure **subskrypcji** , które mają być używane.

6. Na liście rozwijanej **Grupa zasobów** wybierz tej samej grupy zasobów, utworzony wcześniej.

4. Na liście rozwijanej **Plan usług aplikacji** wybierz tego samego planu, który został utworzony wcześniej. 

7. Kliknij przycisk **Utwórz**.

    Programu Visual Studio tworzy aplikacji sieci web, tworzy profilu publikowania i wyświetla **połączenia** — krok kreatora **Publikowania w sieci Web** .

    Nie klikaj jeszcze **Publikuj** . W poniższej sekcji można skonfigurować nową aplikację sieci web, aby nawiązać połączenie aplikację interfejsu API warstwy środkowej, która działa w aplikacji usługi. 

### <a name="set-the-middle-tier-url-in-web-app-settings"></a>Ustawianie adresu URL warstwy środkowej w obszarze Ustawienia aplikacji sieci web

1. Przejdź do [portalu Azure](https://portal.azure.com/), a następnie przejdź do karta **Aplikacji sieci Web** dla aplikacji sieci web, utworzone przez Ciebie do obsługi projektu TodoListAngular (fronton).

2. Kliknij pozycję **Ustawienia > Ustawienia aplikacji**.

3. W sekcji **Ustawienia aplikacji** Dodaj następujący klucz i wartość:

  	|Klawisz|Wartość|Przykład
  	|---|---|---|
  	|toDoListAPIURL|Nazwa aplikacji warstwa środkowa interfejsu API https://{your} .azurewebsites .net|https://todolistapi0121.azurewebsites.NET|

4. Kliknij przycisk **Zapisz**.

    Po uruchomieniu kodu platformy Azure wartość ta zastępuje adres URL hosta lokalnego, który znajduje się w pliku *Web.config* . 

    Kod, który pobiera wartość ustawienia znajduje się w *index.cshtml*:

        <script type="text/javascript">
            var apiEndpoint = "@System.Configuration.ConfigurationManager.AppSettings["toDoListAPIURL"]";
        </script>
        <script src="app/scripts/todoListSvc.js"></script>

    Kod w *todoListSvc.js* używa ustawień:

        return {
            getItems : function(){
                return $http.get(apiEndpoint + '/api/TodoList');
            },
            getItem : function(id){
                return $http.get(apiEndpoint + '/api/TodoList/' + id);
            },
            postItem : function(item){
                return $http.post(apiEndpoint + '/api/TodoList', item);
            },
            putItem : function(item){
                return $http.put(apiEndpoint + '/api/TodoList/', item);
            },
            deleteItem : function(id){
                return $http({
                    method: 'DELETE',
                    url: apiEndpoint + '/api/TodoList/' + id
                });
            }
        };

### <a name="deploy-the-todolistangular-web-project-to-the-new-web-app"></a>Wdrażanie ToDoListAngular projektu sieci web do nowej aplikacji sieci web

*  W programie Visual Studio w **połączenia** kroku kreatora **Publikowanie sieci Web** kliknij pozycję **Publikuj**.

    Program Visual Studio wdraża projektu ToDoListAngular w nowej aplikacji sieci web i otwarcie przeglądarki do określonego adresu URL aplikacji sieci web. 

### <a name="test-the-application-without-cors-enabled"></a>Przetestuj aplikację bez CORS włączony 

2. W przeglądarce narzędzi dla deweloperów Otwórz okno konsoli.

3. W oknie przeglądarki, który jest wyświetlany interfejs użytkownika AngularJS kliknij łącze **Listy zadań do wykonania** .

    Kod JavaScript próbuje połączeń aplikacji warstwa środkowa interfejsu API, ale połączenie kończy się niepowodzeniem, ponieważ zewnętrznej działa w domenie innej niż wewnętrzna. Okno projektanta narzędzia konsoli w przeglądarce zawiera komunikat o błędzie origin między.

    ![Komunikat o błędzie origin krzyżowe](./media/app-service-api-cors-consume-javascript/consoleaccessdenied.png)

## <a name="configure-cors-for-the-middle-tier-api-app"></a>Konfigurowanie CORS aplikacji interfejsu API inicjał drugiego poziomu

W tej sekcji można skonfigurować ustawienie CORS platformy Azure warstwa środkowa interfejsu API ToDoListAPI aplikacji. To ustawienie umożliwia warstwa środkowa aplikacji interfejsu API do odbierania połączeń JavaScript za pomocą aplikacji sieci web, która utworzone dla projektu ToDoListAngular.

8. W przeglądarce przejdź do [portalu Azure](https://portal.azure.com/).

2. Kliknij pozycję **Usługi aplikacji**, a następnie kliknij aplikację interfejsu API ToDoListAPI (inicjał drugiego poziomu).

    ![Wybierz aplikację interfejsu API w portalu](./media/app-service-api-cors-consume-javascript/browseapiapps.png)

10. W kartę **Ustawienia** , która zostanie otwarta po prawej stronie karta **interfejsu API aplikacji** Znajdź sekcję **interfejsu API** , a następnie kliknij **CORS**.

    ![Wybierz pozycję CORS w portalu](./media/app-service-api-cors-consume-javascript/clicksettings.png)

12. W polu tekstowym wprowadź adres URL aplikacji sieci web ToDoListAngular (fronton). Na przykład jeśli wdrożono projektu ToDoListAngular do aplikacji sieci web o nazwie todolistangular0121, Zezwól na połączenia z adresu URL `https://todolistangular0121.azurewebsites.net`.

    Alternatywnie możesz wprowadzić gwiazdka (*), aby określić, że wszystkie domeny pochodzenia są akceptowane.

13. Kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz](./media/app-service-api-cors-consume-javascript/corsinportal.png)

    Po kliknięciu przycisku **Zapisz**aplikacji interfejsu API akceptuje JavaScript połączenia z określonego adresu URL. W tym zrzut ekranu przedstawiający aplikację ToDoListAPI0223 interfejsu API akceptuje połączeń klienta kodu JavaScript w aplikacji sieci web ToDoListAngular.

### <a name="test-the-application-with-cors-enabled"></a>Przetestuj aplikację z CORS włączony

* Otwórz przeglądarkę HTTPS adres URL aplikacji sieci web. 

    Teraz aplikacja umożliwia wyświetlanie, dodawanie, edytowanie i usuwanie elementów do wykonania. 

    ![Lista zadań do wykonania strony przykładowe aplikacji](./media/app-service-api-cors-consume-javascript/corssuccess.png)

## <a name="app-service-cors-versus-web-api-cors"></a>Aplikacja usługi CORS a CORS interfejsu API sieci Web

W projekcie interfejs API sieci Web możesz zainstalować pakiet [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) NuGet, aby uwzględnić w kodzie domen akceptuje z interfejsu API języka JavaScript nawiąże połączenie z.
 
Obsługa CORS interfejsu API sieci Web jest bardziej elastyczne niż aplikacja CORS usługi pomocy technicznej. Na przykład w kodzie można określić różnych miejsc pochodzenia zaakceptowanych, metod inną akcję, gdy dla aplikacji usługi CORS Określ jeden zbiór zaakceptowanych miejsc pochodzenia dla wszystkich metod aplikację interfejsu API.

> [AZURE.NOTE] Nie próbuj używać zarówno CORS interfejsu API sieci Web i aplikacji usługi CORS w jedną aplikację interfejsu API. Aplikacji usługi CORS będą miały pierwszeństwo i CORS interfejsu API sieci Web wywoła żadnych skutków. Na przykład jeśli włączone jedną domenę origin w aplikacji usługi, a następnie Włącz wszystkie domeny pochodzenia w kodzie interfejs API sieci Web, aplikacji interfejsu API Azure tylko akceptuje połączenia z domeny określonej w Azure.

### <a name="how-to-enable-cors-in-web-api-code"></a>Jak włączyć CORS w kodzie interfejs API sieci Web

Poniższe kroki podsumowywanie proces Włączanie obsługi CORS interfejsu API sieci Web. Aby uzyskać więcej informacji zobacz [Włączanie żądania Origin krzyżowe w 2 interfejsu API sieci Web programu ASP.NET](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api).

1. W projekcie interfejs API sieci Web należy zainstalować pakiet NuGet [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) .

1. Dołączanie `config.EnableCors()` wiersza kodu w metodzie **zarejestrować** klasy **WebApiConfig** , tak jak w poniższym przykładzie. 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. W kontrolerze interfejs API sieci Web, Dodaj `using` poufności informacji dotyczące `System.Web.Http.Cors` nazw i Dodaj `EnableCors` atrybut klasy kontroler lub metod poszczególnych akcji. W poniższym przykładzie pomocy technicznej CORS dotyczą całej kontrolerze.

        namespace ToDoListAPI.Controllers 
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"https://todolistangular0121.azurewebsites.net", headers:"accept,content-type,origin,x-my-header", methods: "get,post")]
            public class ToDoListController : ApiController
 
## <a name="using-azure-api-management-with-api-apps"></a>Zarządzanie Azure API za pomocą interfejsu API aplikacji

Jeśli używasz usługi zarządzania interfejsu API Azure z aplikacji interfejsu API skonfigurować CORS w zarządzaniu interfejsu API zamiast w aplikacji interfejsu API. Aby uzyskać więcej informacji zobacz następujące zasoby:

* [Omówienie zarządzania interfejsu API Azure (klip wideo: CORS zaczyna się od 12:10)](https://azure.microsoft.com/documentation/videos/azure-api-management-overview/)
* [Interfejs API zarządzania krzyżowe zasady domeny](https://msdn.microsoft.com/library/azure/dn894084.aspx#CORS)
 
## <a name="troubleshooting"></a>Rozwiązywanie problemów

W przypadku, gdy napotkasz problem podczas przechodzenia do tego samouczka, Oto kilka pomysłów dotyczących rozwiązywania problemów.

* Upewnij się, że korzystasz z najnowszą wersją pakietu [SDK Azure dla środowiska .NET dla programu Visual Studio 2015 r](http://go.microsoft.com/fwlink/?linkid=518003).

* Upewnij się, że wprowadzone `https` ustawienie CORS i upewnij się, że używasz `https` do uruchamiania aplikacji frontonu sieci web.

* Upewnij się, że wprowadzone ustawienie CORS w aplikacji interfejsu API warstwy środkowej, a nie w aplikacji frontonu sieci web.

* Konfiguracji CORS zarówno kod aplikacji i Azure aplikacji usługi, Zauważ, że ustawienie aplikacji usługi CORS zastąpi, niezależnie od wykonywanych w kodzie aplikacji. 

Aby dowiedzieć się więcej na temat programu Visual Studio funkcje, które uproszczenia rozwiązywania problemów, zobacz [Rozwiązywanie problemów z Azure aplikacji usługi aplikacji w programie Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Następne kroki 

W tym artykule pokazano jak włączyć obsługę aplikacji usługi CORS tak, aby połączenie interfejsu API kodu JavaScript klienta w innej domeny. Aby dowiedzieć się więcej o aplikacjach interfejsu API, przeczytaj [Wprowadzenie do uwierzytelniania w aplikacji usługi](../app-service/app-service-authentication-overview.md), a następnie przejdź do samouczka [Uwierzytelnianie użytkownika w przypadku aplikacji interfejsu API](app-service-api-dotnet-user-principal-auth.md) .
