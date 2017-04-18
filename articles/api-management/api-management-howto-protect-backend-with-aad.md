<properties
    pageTitle="Jak chronić interfejs API sieci Web wewnętrznej bazy danych z usługi Azure Active Directory i zarządzania interfejsu API"
    description="Dowiedz się, jak chronić interfejs API sieci Web wewnętrznej bazy danych z usługi Azure Active Directory i zarządzania interfejsu API." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Jak chronić interfejs API sieci Web wewnętrznej bazy danych z usługi Azure Active Directory i zarządzania interfejsu API

Poniższym klipie wideo przedstawiono sposób tworzenia wewnętrznej bazie danych sieci Web interfejsu API i chronić przy użyciu protokołu OAuth 2.0 z usługi Azure Active Directory i zarządzania interfejsu API.  Ten artykuł zawiera omówienie oraz informacje dodatkowe kroki w klipie wideo. Ten 24 minutowym klipie wideo pokazano, jak do:

-   Utwórz wewnętrzną interfejs API sieci Web i zabezpieczyć przy użyciu AAD - począwszy od 1:30
-   Importowanie API do zarządzania interfejsu API — począwszy od 7:10
-   Konfigurowanie portalu Deweloper nawiązać połączenie z interfejsu API — począwszy od 9:09
-   Konfigurowanie aplikacji komputerowych nawiązać połączenie z interfejsu API — począwszy od 18:08
-   Konfigurowanie zasad sprawdzania poprawności JWT, aby autoryzować wstępnie żądania - począwszy od 20:47

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Utwórz katalog Azure AD

Aby zabezpieczyć interfejsu API usług sieci Web kopii przy użyciu usługi Azure Active Directory, musisz najpierw mieć dzierżawę AAD. W tym klipie wideo o nazwie **APIMDemo** dzierżawy jest używany. Aby utworzyć dzierżawę AAD, zaloguj się do [Portalu klasyczny Azure](https://manage.windowsazure.com) i kliknij przycisk **Nowy**->**Aplikacji usług**->**Usługi Active Directory**->**katalogu**->**Utworzyć niestandardowe**. 

![Azure Active Directory][api-management-create-aad-menu]

W tym przykładzie katalogu o nazwie **APIMDemo** jest tworzony z domeną domyślną o nazwie **DemoAPIM.onmicrosoft.com**. Ten katalog jest używany w całej wideo.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Tworzenie usługi interfejs API sieci Web zabezpieczonych przez usługi Azure Active Directory

W tym kroku wewnętrznej bazie danych interfejs API sieci Web jest tworzona przy użyciu programu Visual Studio 2013. W tym kroku wideo zaczyna się od 1:30. Aby utworzyć interfejs API sieci Web kliknij pozycję **plik**wewnętrznej bazy danych projektu w programie Visual Studio->**Nowy**->**projektu**i wybierz **Aplikację sieci Web programu ASP.NET** z listy szablonów **w sieci Web** . W tym klipie wideo projekt ma nazwę **APIMAADDemo**. Kliknij przycisk **OK** , aby utworzyć projekt. 

![Programu Visual Studio][api-management-new-web-app]

Kliknij **Interfejs API sieci Web** z **Wybierz listę szablonów** do tworzenia projektu interfejs API sieci Web. Aby skonfigurować uwierzytelnianie katalogu Azure kliknij **Zmień uwierzytelniania**.

![Nowy projekt][api-management-new-project]

Kliknij pozycję **Konta organizacji**, a następnie określ **domenę** Twojej dzierżawy AAD. W tym przykładzie domena jest **DemoAPIM.onmicrosoft.com**. Domena katalogu można znaleźć na karcie **Domains** katalogu.

![Domeny][api-management-aad-domains]

Skonfiguruj odpowiednie ustawienia w oknie dialogowym **Zmienianie uwierzytelniania** , a następnie kliknij **przycisk OK**.

![Zmienianie uwierzytelniania][api-management-change-authentication]

Po kliknięciu **przycisku OK** Visual Studio spróbuje zarejestrować aplikację z katalogu Azure AD i może zostać wyświetlony monit zalogować się przez program Visual Studio. Zaloguj się przy użyciu konta administratora dla katalogu.

![Zaloguj się do programu Visual Studio][api-management-sign-in-vidual-studio]

Aby skonfigurować tego projektu jako Azure interfejs API sieci Web zaznacz pole wyboru dla **hosta w chmurze** , a następnie kliknij **przycisk OK**.

![Nowy projekt][api-management-new-project-cloud]

Może zostać wyświetlony monit do zalogowania się do Azure, a następnie można skonfigurować aplikacji sieci Web.

![Konfigurowanie][api-management-configure-web-app]

W tym przykładzie określono nowy **plan usług aplikacji** o nazwie **APIMAADDemo** .

Kliknij **przycisk OK** , aby skonfigurować aplikację sieci Web i Utwórz projekt.

## <a name="add-the-code-to-the-web-api-project"></a>Dodawanie kodu do projektu interfejs API sieci Web

Następnym krokiem w klipie wideo dodaje kod do projektu interfejs API sieci Web. W tym kroku zaczyna się od 4:35.

Interfejs API sieci Web, w tym przykładzie zawiera usługę prostego kalkulatora przy użyciu modelu i kontrolerze. Aby dodać model usługi, kliknij prawym przyciskiem myszy **modeli** w **Eksploratorze rozwiązań** i wybierz przycisk **Dodaj**, **zajęć**. Nazwa klasy `CalcInput` i kliknij przycisk **Dodaj**.

Dodaj następujący `using` instrukcji na początek `CalcInput.cs` pliku.

    using Newtonsoft.Json;

 Zamień wygenerowane klasy poniższy kod.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Kliknij prawym przyciskiem myszy **kontrolery** w **Eksploratorze rozwiązań** i wybierz pozycję **Dodaj**->**kontrolerze**. Wybierz **Kontroler 2 interfejsu API sieci Web — puste** , a następnie kliknij przycisk **Dodaj**. Wpisz nazwę kontrolera **CalcController** i kliknij przycisk **Dodaj**.

![Dodawanie kontrolera][api-management-add-controller]

Dodaj następujący `using` instrukcji na początek `CalcController.cs` pliku.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

Zamień klasy wygenerowane kontroler poniższy kod. Wykonuje kod `Add`, `Subtract`, `Multiply`, i `Divide` operacji podstawowe interfejsu API kalkulatora.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Naciśnij klawisz **F6** , aby utworzyć i sprawdź, czy rozwiązanie.

## <a name="publish-the-project-to-azure"></a>Publikowanie projektu w Azure

W tym kroku Visual Studio projektu są publikowane w usłudze Azure. W tym kroku wideo zaczyna się od 5:45.

Aby opublikować projekt Azure, kliknij prawym przyciskiem myszy **APIMAADDemo** projektu w programie Visual Studio, a następnie wybierz pozycję **Publikuj**. Zachowaj ustawienia domyślne w oknie dialogowym **Publikowanie sieci Web** , a następnie kliknij pozycję **Publikuj**.

![Publikowanie w sieci Web][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Udzielanie uprawnień do wewnętrznej bazy danych aplikacji usługi Azure AD

Nowa aplikacja usługi wewnętrznej bazy danych zostanie utworzona w katalogu Azure AD w ramach procesu konfigurowania i publikowania projektu interfejs API sieci Web. W tym kroku wideo, zaczynając od 6:13 uprawnienia są do wewnętrznej bazy danych interfejs API sieci Web.

![Aplikacji][api-management-aad-backend-app]

Kliknij nazwę aplikacji, aby skonfigurować wymagane uprawnienia. Przejdź na kartę **Konfiguruj** i przewiń w dół do sekcji **uprawnienia do innych aplikacji** . Kliknij przycisk **Uprawnienia aplikacji** listy rozwijanej obok usługi **Windows** **Azure Active Directory**, zaznacz pole wyboru obok **Odczyt katalogu danych**i kliknij przycisk **Zapisz**.

![Dodaj uprawnienia][api-management-aad-add-permissions]

>[AZURE.NOTE] Jeśli usługi **Windows** **Azure Active Directory** nie znajduje się w obszarze uprawnienia do innych aplikacji, kliknij pozycję **Dodaj aplikację** i dodaj go z listy.

Zanotuj **Aplikacji identyfikator URI** do użycia w kolejnych czynności Jeśli aplikacja Azure AD jest skonfigurowany do portalu zarządzania interfejsu API Deweloper.

![Aplikacja identyfikator URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Importowanie interfejsu API sieci Web do zarządzania interfejsu API

Interfejsy API są skonfigurowane w portalu programu publisher interfejsu API jest dostępna za pośrednictwem portalu klasyczny Azure. Aby uzyskać dostęp do portalu programu publisher, kliknij przycisk **Zarządzaj** w portalu klasyczny Azure dotyczące usługi zarządzania interfejsu API. Jeśli wystąpienie usługi zarządzania interfejsu API nie został jeszcze utworzony, zobacz [Tworzenie wystąpienia usługi zarządzania interfejsu API][] w samouczku [Zarządzanie do pierwszego interfejsu API][] .

![Portal programu Publisher][api-management-management-console]

Operacje można [ręcznie dodawać do interfejsów API usługi](api-management-howto-add-operations.md)lub mogą być importowane. W tym klipie wideo operacje zostaną zaimportowane w formacie Swagger, zaczynając od 6:40.

Tworzenie pliku o nazwie `calcapi.json` z następujące zagadnienia i zapisz go na komputerze. Upewnij się, że `host` atrybutów punkty do swojego interfejsu API usług Web wewnętrznej bazy danych. W tym przykładzie `"host": "apimaaddemo.azurewebsites.net"` jest używana.

{"swagger": "2.0", "o": {"tytuł": "Kalkulatora", "opis": "Arithmetics za pomocą protokołu HTTP!", "wersji": "1.0"}, "host": "apimaaddemo.azurewebsites.net", "nieistniejący": "i interfejs api", "schematy": ["http"], "ścieżki": {"/ add? = {} i b = {b}": {"Pobierz": {"opis": "Odpowiada z funkcją Suma dwóch liczb.", "operationId": "Dodaj dwóch liczb całkowitych", "Parametry": [{"Nazwa": "a", "w": "kwerendy", "opis": "pierwszym argumentem. Wartość domyślna to <code>51</code>. ","wymagane": true"wartość domyślna":"51","stałego": ["51"]}, {"Nazwa":"b","w":"kwerendy","opis":"drugi argument. Wartość domyślna to <code>49</code>. ","wymagane": true"wartość domyślna":"49","stałego": ["49"]}],"odpowiedzi": {}}}," / sub?a = {} & b = {b} ": {"Pobierz": {"opis":"Odpowiada przy różnica dwóch liczb.","operationId":"Odejmij dwóch liczb całkowitych","Parametry": [{"Nazwa":"a","w":"kwerendy","opis":"pierwszym argumentem. Wartość domyślna to <code>100</code>. ","wymagane": true"wartość domyślna":"100","stałego": ["100"]}, {"Nazwa":"b","w":"kwerendy","opis":"drugi argument. Wartość domyślna to <code>50</code>. ","wymagane": true"wartość domyślna":"50","stałego": ["50"]}],"odpowiedzi": {}}}," / div?a = {} & b = {b} ": {"Pobierz": {"opis":"Odpowiada przy iloraz dwóch liczb.","operationId":"Dzielenie dwóch liczb całkowitych","Parametry": [{"Nazwa":"a","w":"kwerendy","opis":"pierwszym argumentem. Wartość domyślna to <code>100</code>. ","wymagane": true"wartość domyślna":"100","stałego": ["100"]}, {"Nazwa":"b","w":"kwerendy","opis":"drugi argument. Wartość domyślna to <code>20</code>. ","wymagane": true"wartość domyślna":"20","stałego": ["20"]}],"odpowiedzi": {}}}," / mul?a = {} & b = {b} ": {"Pobierz": {"opis":"Odpowiada przy iloczyn dwóch liczb.","operationId":"Mnożenie dwóch podanych liczb całkowitych","Parametry": [{"Nazwa":"a","w":"kwerendy","opis":"pierwszym argumentem. Wartość domyślna to <code>20</code>. ","wymagane": true"wartość domyślna":"20","stałego": ["20"]}, {"Nazwa":"b","w":"kwerendy","opis":"drugi argument. Wartość domyślna to <code>5</code>. ","wymagane": true"wartość domyślna":"5","stałego": ["5"]}],"odpowiedzi": {}}}}}

Aby zaimportować kalkulatora interfejsu API, kliknij pozycję **interfejsy API** z **Interfejsu API zarządzania** menu po lewej stronie, a następnie kliknij **Importowanie interfejsu API**.

![Przycisk Importuj interfejsu API][api-management-import-api]

Wykonaj poniższe czynności, aby skonfigurować kalkulatora interfejsu API.

1. Kliknij pozycję **z pliku**, przejdź do `calculator.json` zostanie zapisany plik, a następnie kliknij przycisk radiowy **Swagger** .
2. Wpisz **obliczenia** w polu tekstowym **sufiks adresu URL interfejsu API sieci Web** .
3. Kliknij w polu **produktów (opcjonalnie)** i wybierz **Starter**.
4. Kliknij przycisk **Zapisz** do zaimportowania API.

![Dodawanie nowego interfejsu API usługi][api-management-import-new-api]

Po zaimportowaniu API stronę podsumowania API jest wyświetlany w portalu programu publisher.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Połączenie API pomyślnie z portalu — Deweloper

W tym momencie API zaimportowano do zarządzania interfejsu API, ale nie jeszcze można wywołać pomyślnie z portalu Deweloper ponieważ usługa wewnętrznej bazy danych są chronione za pomocą uwierzytelniania Azure AD. To przedstawiono w klipie wideo, zaczynając od 7:40, wykonując poniższe czynności.

Kliknij opcję **dzięki portalowi deweloperów** z w prawej górnej części portalu programu publisher.

![Dzięki portalowi deweloperów][api-management-developer-portal-menu]

Kliknij **interfejsy API** i kliknij **kalkulatora** interfejsu API.

![Dzięki portalowi deweloperów][api-management-dev-portal-apis]

Kliknij pozycję **Wypróbuj**.

![Wypróbuj][api-management-dev-portal-try-it]

Kliknij przycisk **Wyślij** i zanotuj stan odpowiedzi **401 Unauthorized**.

![Wyślij][api-management-dev-portal-send-401]

Żądanie nie jest autoryzowany, ponieważ interfejs API wewnętrznej bazy danych jest chroniony przez usługi Azure Active Directory. Przed pomyślnie nawiązywania połączeń z interfejsu API projektanta portalu należy skonfigurować Aby autoryzować deweloperów przy użyciu OAuth 2.0. W poniższych sekcjach opisano ten proces.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Zarejestruj się w portalu Deweloper jako aplikacja AAD

Pierwszy krok w konfigurowaniu portalu Deweloper, aby autoryzować deweloperów przy użyciu OAuth 2.0 jest zarejestrować portalu Deweloper jako aplikacja AAD. Jest to wykazać, zaczynając od 8:27 w klipie wideo.

Przejdź do dzierżawy Azure AD w pierwszym kroku ten klip wideo, w tym przykładzie **APIMDemo** i przejdź do karty **aplikacji** .

![Nowa aplikacja][api-management-aad-new-application-devportal]

Kliknij przycisk **Dodaj** , aby utworzyć nową aplikację usługi Azure Active Directory, a następnie wybierz pozycję **Dodaj aplikację, którą rozwija się w mojej organizacji**.

![Nowa aplikacja][api-management-new-aad-application-menu]

Wybieranie **aplikacji sieci Web i/lub interfejs API sieci Web**, wprowadź nazwę i kliknij strzałkę w następnym. W tym przykładzie jest używany **APIMDeveloperPortal** .

![Nowa aplikacja][api-management-aad-new-application-devportal-1]

Dla **logowania jednokrotnego adresu URL** wprowadź adres URL usługi zarządzania interfejsu API i dołączanie `/signin`. W tym przykładzie jest używany **https://contoso5.portal.azure-api.net/signin **.

Dla **Adresu URL identyfikator aplikacji** wpisz adres URL usługi zarządzania interfejsu API i Dołącz niektórych unikatowe znaki. Mogą one zostać wszelkie odpowiednie znaki i w tym przykładzie jest używany **https://contoso5.portal.azure-api.net/dp** . Gdy odpowiednie **Właściwości aplikacji** zostały skonfigurowane, kliknij znacznik wyboru, aby utworzyć aplikację.

![Nowa aplikacja][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Konfigurowanie serwera autoryzacji interfejsu API zarządzania OAuth 2.0

Następnym krokiem jest skonfigurować serwer autoryzacji OAuth 2.0 w zarządzaniu interfejsu API. W tym kroku przedstawiono w klipie wideo, począwszy od 9:43.

W menu zarządzania interfejsu API po lewej stronie kliknij pozycję **Zabezpieczenia** , kliknij pozycję **OAuth 2.0**, a następnie kliknij **Dodaj autoryzacji** serwera.

![Dodawanie autoryzacji serwera][api-management-add-authorization-server]

Wprowadź nazwę i opcjonalny opis w polach **Nazwa** i **Opis** . Te pola są używane do identyfikowania serwera autoryzacji OAuth 2.0 w wystąpieniu usług zarządzania interfejsu API. W tym przykładzie jest używany **autoryzacji serwera pokaz** . Później podczas określania server OAuth 2.0 może być używany do uwierzytelniania dla interfejsu API wybierzesz tę nazwę.

Uzyskać **adres URL strony rejestracji klienta** wprowadzać wartości symbolu zastępczego, takie jak `http://localhost`.  **Adres URL strony rejestracji klienta** punktów do strony, którą użytkownicy mogą używać do tworzenia i konfigurowania własnych kont dla OAuth 2.0 do grupy dostawców obsługujących zarządzanie kontami użytkowników. W tym przykładzie użytkownicy nie tworzenie i konfigurowanie własnymi kontami, aby służy symbolu zastępczego.

![Dodawanie autoryzacji serwera][api-management-add-authorization-server-1]

Następnie określ **adres URL punktu końcowego autoryzacji** i **Token adres URL punktu końcowego**.

![Autoryzacja serwera][api-management-add-authorization-server-1a]

Te wartości mogą być pobierane na stronie **Punkty końcowe aplikacji** utworzoną portalu Deweloper aplikacji AAD. Do programu access punkty końcowe przejdź na kartę **Konfigurowanie** aplikacji AAD i kliknij przycisk **punkty końcowe widoku**.

![Aplikacji][api-management-aad-devportal-application]

![Widok punkty końcowe][api-management-aad-view-endpoints]

Skopiuj punkt **końcowy autoryzacji OAuth 2.0** i wkleić go w polu tekstowym **adres URL punktu końcowego autoryzacji** .

![Dodawanie autoryzacji serwera][api-management-add-authorization-server-2]

Skopiuj punkt **końcowy token OAuth 2.0** i wkleić go w polu tekstowym **adres URL punktu końcowego tokenu** .

![Dodawanie autoryzacji serwera][api-management-add-authorization-server-2a]

Oprócz wklejenie token punktu końcowego, dodać parametr dodatkowe treści o nazwie **zasobu** i do użytku wartość **Aplikacji identyfikator URI** z poziomu aplikacji AAD usługi wewnętrznej bazy danych, który został utworzony, kiedy projekt Visual Studio został opublikowany.

![Aplikacja identyfikator URI][api-management-aad-sso-uri]

Następnie określ poświadczenia klienta. Są to poświadczenia dla zasobów, które mają dostęp, w tym przypadku usługę wewnętrznej bazy danych.

![Poświadczenia klienta][api-management-client-credentials]

Aby uzyskać **Identyfikator klienta**, przejdź do karty **Konfigurowanie** aplikacji AAD usługi wewnętrznej bazy danych i skopiuj **Identyfikator klienta**.

Aby uzyskać kliknij **Tajny klienta** , **Wybierz czas trwania** listy rozwijanej w sekcji **klawiszy** i określ interwał. W tym przykładzie jest używany 1 rok.

![Identyfikator klienta][api-management-aad-client-id]

Kliknij przycisk **Zapisz** , aby zapisać konfigurację i wyświetlić klucz. 

>[AZURE.IMPORTANT] Zanotuj tego klucza. Po zamknięciu okna Konfiguracja usługi Azure Active Directory klucz nie można wyświetlić ponownie.

Kopiowanie klucz do Schowka, powróć do portalu programu publisher, Wklej klucz w polu tekstowym **Hasło klienta** i kliknij przycisk **Zapisz**.

![Dodawanie autoryzacji serwera][api-management-add-authorization-server-3]

Natychmiast po poświadczeń klienta jest udzielanie kodu autoryzacji. Skopiuj kod autoryzacji i wrócić do aplikacji portalu Deweloper Azure AD Konfigurowanie strony i Wklej zezwolenie udzielanie w polu **Adres URL odpowiedzi** , a następnie kliknij przycisk **Zapisz** .

![Adres URL odpowiedzi][api-management-aad-reply-url]

Następnym krokiem jest skonfigurowanie uprawnień dla portalu Deweloper aplikacji AAD. Kliknij pozycję **Uprawnienia aplikacji** , a następnie zaznacz pole wyboru obok **Odczyt katalogu danych**. Kliknij przycisk **Zapisz** , aby zapisać zmiany, a następnie kliknij pozycję **Dodaj aplikację**.

![Dodaj uprawnienia][api-management-add-devportal-permissions]

Kliknij ikonę Wyszukaj, wpisz **APIM** na początek pola, wybierz pozycję **APIMAADDemo**i kliknij znacznik wyboru, aby zapisać.

![Dodaj uprawnienia][api-management-aad-add-app-permissions]

Kliknij pozycję **Delegowane uprawnienia** dla **APIMAADDemo** i zaznacz pole wyboru obok **APIMAADDemo programu Access**i kliknij przycisk **Zapisz**. Dzięki temu deweloper aplikacji portalu, aby uzyskać dostęp do usług wewnętrznej bazy danych.

![Dodaj uprawnienia][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Włączanie OAuth 2.0 uwierzytelnienia użytkownika dla interfejsu API kalkulatora

Teraz, gdy serwer OAuth 2.0 jest skonfigurowany, można określić ją w ustawień zabezpieczeń dla swojego interfejsu API. W tym kroku przedstawiono w klipie wideo, zaczynając od 14:30.

Kliknij pozycję **interfejsy API** w menu po lewej stronie, a następnie kliknij przycisk **kalkulatora** do wyświetlania i konfigurowania ustawień.

![Kalkulator interfejsu API][api-management-calc-api]

Przejdź do karty **zabezpieczeń** , zaznacz pole wyboru **OAuth 2.0** , wybierz serwer odpowiedniej autoryzacji z listy rozwijanej **autoryzacji serwera** i kliknij przycisk **Zapisz**.

![Kalkulator interfejsu API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Pomyślne wywołanie interfejsu API kalkulatora z portalu — Deweloper

Teraz, gdy autoryzacji OAuth 2.0 jest skonfigurowana w interfejsie API, operacjach może pomyślnie wywołana z Centrum deweloperów. W tym kroku przedstawiono w klipie wideo, zaczynając od 15:00.

Przejść z powrotem do wykonywania **dwóch liczb całkowitych Dodaj** usług kalkulatora w portalu Deweloper, a następnie kliknij pozycję **Wypróbuj**. Uwaga nowy element w sekcji **autoryzacji** odpowiadające do właśnie dodanego serwera autoryzacji.

![Kalkulator interfejsu API][api-management-calc-authorization-server]

Z listy rozwijanej autoryzacji wybierz **Kod autoryzacji** i wprowadź poświadczenia konta, które zostanie użyte. Po zalogowaniu się do konta może nie zostać wyświetlony monit.

![Kalkulator interfejsu API][api-management-devportal-authorization-code]

Kliknij przycisk **Wyślij** i zanotuj **Stan odpowiedzi** **200 OK** i wyniki operacji w zawartości odpowiedzi.

![Kalkulator interfejsu API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Konfigurowanie aplikacji komputerowych nawiązać połączenie z interfejsu API

Następnej procedury w klipie wideo zaczyna się od 16:30 i konfiguruje prostych aplikacji klasycznej nawiązać połączenie z interfejsu API. Pierwszym krokiem jest rejestrowania aplikacji klasycznej w Azure AD i nadaj mu dostęp do katalogu i usługą wewnętrznej bazy danych. Na 18:25 istnieje pokaz aplikacji klasycznej wywołanie operacji na kalkulatora interfejsu API.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Konfigurowanie zasad sprawdzania poprawności JWT, aby autoryzować wstępnie żądania

Ostatecznej procedury w klipie wideo zaczyna się od 20:48 i pokazano, jak za pomocą zasad [Sprawdzania poprawności JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) wstępnie autoryzować żądania sprawdzając tokeny dostępu każdego przychodzącego żądania. Jeśli żądanie nie jest sprawdzana poprawność przez zasady sprawdzania poprawności JWT, żądanie jest blokowany przez interfejs API zarządzania i nie jest przekazywany wzdłuż do wewnętrznej bazy danych.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Aby zobaczyć inną pokaz Konfigurowanie i używanie tych zasad, zobacz [177 odcinka pokrycia w chmurze: więcej funkcji zarządzania interfejsu API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) i szybkie przewijanie do przodu do 13:50. Szybkie przejście do 15:00, aby wyświetlić zasady skonfigurowane w edytorze zasad, a następnie do 18:50, aby zobaczyć pokaz wywołanie operacji z portalu Deweloper zarówno z i bez token autoryzacji wymagane.

## <a name="next-steps"></a>Następne kroki
-   Zapoznaj się z więcej [klipów wideo](https://azure.microsoft.com/documentation/videos/index/?services=api-management) dotyczących zarządzania interfejsu API.
-   Inne sposoby bezpiecznego usługi wewnętrznej bazy danych, zobacz [wzajemnego certyfikatu uwierzytelniania](api-management-howto-mutual-certificates.md) i [Łączenie za pośrednictwem sieci VPN lub ExpressRoute](api-management-howto-setup-vpn.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Tworzenie wystąpienia usługi zarządzania interfejsu API]: api-management-get-started.md#create-service-instance
[Zarządzanie do pierwszego interfejsu API]: api-management-get-started.md
