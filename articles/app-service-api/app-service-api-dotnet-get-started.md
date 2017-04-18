<properties
    pageTitle="Wprowadzenie do aplikacji interfejsu API i ASP.NET w aplikacji usługi | Microsoft Azure"
    description="Dowiedz się, jak tworzenie, wdrażanie i używanie aplikacji interfejsu API programu ASP.NET w usłudze Azure aplikacji przy użyciu programu Visual Studio 2015 r."
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
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="rachelap"/>

# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a>Wprowadzenie do aplikacji interfejsu API, ASP.NET i Swagger Azure aplikacji usługi

[AZURE.INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

Jest to pierwsza seria samouczki, które przedstawiono sposoby używania funkcji Azure aplikacji usługi, które są przydatne podczas opracowywania i obsługi RESTful interfejsów API.  Ten samouczek obejmuje obsługę interfejsu API metadanych w formacie Swagger.

Opisano następujące zagadnienia:

* Jak tworzyć i wdrażać [aplikacje interfejsu API](app-service-api-apps-why-best-platform.md) usługi aplikacji Azure za pomocą narzędzi wbudowanych w program Visual Studio 2015 r.
* Jak zautomatyzować odnajdowania interfejsu API przy użyciu pakietu Swashbuckle NuGet do dynamicznego generowania metadanych Swagger interfejsu API.
* Jak umożliwia automatyczne generowanie kodu klienta aplikacji interfejsu API Swagger interfejsu API metadanych.

## <a name="sample-application-overview"></a>Omówienie aplikacji próbki

W tym samouczku pracy z aplikacją Przykładowa lista prostych zadań do wykonania. Aplikacja ma fronton jednostronicowy aplikacji (SPA), warstwa środkowa interfejs API sieci Web programu ASP.NET i warstwa danych interfejs API sieci Web programu ASP.NET.

![Diagram aplikacji przykładowych aplikacji interfejsu API](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

Poniżej przedstawiono zrzut ekranu: fronton [AngularJS](https://angularjs.org/) .

![Interfejs API aplikacji przykładowej aplikacji Lista zadań do wykonania](./media/app-service-api-dotnet-get-started/todospa.png)

Rozwiązania programu Visual Studio obejmuje trzy projekty:

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* **ToDoListAngular** - fronton: bezpieczne uwierzytelnianie HASŁA AngularJS, wywołuje inicjał drugiego poziomu.

* **ToDoListAPI** - warstwa środkowa: projekt interfejsu API sieci Web programu ASP.NET, który wymaga warstwy danych do wykonywania operacji OBSŁUGIWAŁ na elementów do wykonania.

* **ToDoListDataAPI** - warstwy danych: interfejs API sieci Web programu ASP.NET projektu, które wykonuje OBSŁUGIWAŁ operacje na elementów do wykonania.

Architektura trzeciej warstwy jest jednym z wielu architektury, które można zaimplementować przy użyciu aplikacji interfejsu API i tutaj służy tylko do celów pokaz. Kod w każdej warstwie jest możliwie najprostsze celu zademonstrowania funkcji interfejsu API aplikacji; na przykład warstwy danych używa pamięci serwera, a nie bazy danych jako jego mechanizmu ważność.

Na ten samouczek będą dostępne dwa projekty interfejs API sieci Web w górę i uruchamiania w chmurze w aplikacjach interfejs API usługi aplikacji.

Następnego samouczka w serii wdraża fronton bezpieczne uwierzytelnianie HASŁA w chmurze.

## <a name="prerequisites"></a>Wymagania wstępne

* Interfejs API sieci Web programu ASP.NET - samouczka instrukcje założono, że masz podstawowe informacje o sposobach przetwarzania ASP.NET [2 interfejsu API sieci Web](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) w programie Visual Studio.

* Konto Azure — możesz [otworzyć konto Azure bezpłatnie](/pricing/free-trial/?WT.mc_id=A261C142F) lub [zalet subskrybentów uaktywnić programu Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

    Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi, aby utworzyć konto Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751). Możesz od razu utworzyć krótkotrwałe starter aplikację w aplikacji usługi — **wymaga karty kredytowej**, a nie zobowiązań.

* Visual Studio 2015 z zestawu [SDK Azure dla środowiska .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) — SDK instaluje Visual Studio 2015 automatycznie, jeśli jeszcze nie masz.

    * W programie Visual Studio, kliknij pozycję Pomoc -> o Microsoft Visual Studio i upewnij się, że "Narzędzia Azure aplikacji usługi v2.9.1" lub nowszym.

    ![Azure vesion narzędzia aplikacji](./media/app-service-api-dotnet-get-started/apiversion.png)

    >[AZURE.NOTE] W zależności od tego, ile zależności SDK jest już na komputerze instalowania zestawu SDK może zająć dużo czasu, na kilka minut pół godziny lub więcej.

## <a name="download-the-sample-application"></a>Pobieranie aplikacji przykładowej

1. Pobierz [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) repozytorium.

    Możesz kliknąć przycisk **Pobierz ZIP** lub klonowanie repozytorium na komputerze lokalnym.

2. Otwórz rozwiązanie listy zadań programu Visual Studio 2015 lub 2013.
   1. Konieczne będzie zaufania każdego z rozwiązań.
        ![Ostrzeżenie o zabezpieczeniach](./media/app-service-api-dotnet-get-started/securitywarning.png)

3. Utworzyć rozwiązanie (CTRL + SHIFT + B) do przywrócenia pakietów NuGet.

    Jeśli chcesz wyświetlić aplikacji w operacji przed wdrożeniem tego, można go uruchamiać lokalnie. Upewnij się, że ToDoListDataAPI jest projektu uruchamiania i uruchom rozwiązanie. Można się spodziewać wyświetlony błąd HTTP 403 w przeglądarce.

## <a name="use-swagger-api-metadata-and-ui"></a>Za pomocą interfejsu użytkownika i Swagger interfejsu API metadanych

Obsługa [Swagger](http://swagger.io/) 2.0 interfejsu API metadanych jest wbudowany w usłudze Azure aplikacji. Każdej aplikacji interfejsu API można określić adres URL punktu końcowego, zwracające metadanych dla interfejsu API w formacie Swagger JSON. Metadane zwrócone przez tego punktu końcowego może służyć do wygenerowania kodu klienta.

Projekt interfejsu API sieci Web programu ASP.NET dynamiczne możliwość wygenerowania metadanych Swagger przy użyciu pakietu NuGet [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) . Pakiet Swashbuckle NuGet jest już zainstalowany w projektach ToDoListDataAPI i ToDoListAPI, które możesz pobrać.

W tej części samouczka przeglądać wygenerowane metadanych Swagger 2.0, i spróbuj test interfejsu użytkownika, który jest oparty na metadanych Swagger.

1. Ustawianie projektu ToDoListDataAPI (**nie** projektu ToDoListAPI) jako projekt startowy.

    ![Ustawianie ToDoDataAPI jako projekt startowy](./media/app-service-api-dotnet-get-started/startupproject.png)

2. Naciśnij klawisz F5 lub kliknij pozycję **Debugowanie > Rozpocznij debugowanie** Aby uruchomić projekt w trybie debugowania.

    W przeglądarce zostanie otwarta i wyświetlenie strony błędu HTTP 403.

3. W pasku adresu przeglądarki Dodaj `swagger/docs/v1` do końca wiersza, a następnie naciśnij klawisz Return. (Adres URL jest `http://localhost:45914/swagger/docs/v1`.)

    To jest domyślny adres URL używane przez Swashbuckle do zwracania metadanych Swagger 2.0 JSON dla interfejsu API.

    Jeśli używasz programu Internet Explorer w przeglądarce zostanie wyświetlony monit o pobranie pliku *v1.json* .

    ![Pobierz JSON metadanych w programie Internet Explorer](./media/app-service-api-dotnet-get-started/iev1json.png)

    Jeśli używasz przeglądarki Chrome, Firefox lub krawędź przeglądarce Wyświetla JSON w oknie przeglądarki. Różnych przeglądarkach obsługiwać JSON inaczej, a okno przeglądarki może wyglądać dokładnie tak jak w przykładzie.

    ![JSON metadanych w programie Chrome](./media/app-service-api-dotnet-get-started/chromev1json.png)

    Następujący przykład przedstawia pierwsza sekcja metadanych Swagger dla interfejsu API, z definicją dla metody Get. Metadane co dyski Swagger interfejsu użytkownika używanego w następującej procedurze, a możesz użyć w dalszej części samouczka automatyczne generowanie kodu klienta.

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },

4. Zamknij przeglądarkę i Zatrzymaj debugowanie programu Visual Studio.

5. W programie project ToDoListDataAPI w **Eksploratorze rozwiązań**Otwórz plik *App_Start\SwaggerConfig.cs* , a następnie przewiń w dół do wiersza 174 i Usuń komentarze poniższy kod.

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    Plik *SwaggerConfig.cs* jest tworzona po zainstalowaniu pakietu Swashbuckle w projekcie. Plik udostępnia kilka sposobów konfigurowania Swashbuckle.

    Kod, który został uncommented umożliwia Swagger interfejsu użytkownika używanego w następującej procedurze. Po utworzeniu projektu interfejs API sieci Web przy użyciu szablonu projektu aplikacji interfejsu API kod jest ujęta w komentarz domyślnie ze względów bezpieczeństwa.

6. Ponownie uruchom projektu.

7. W pasku adresu przeglądarki Dodaj `swagger` do końca wiersza, a następnie naciśnij klawisz Return. (Adres URL jest `http://localhost:45914/swagger`.)

8. Gdy zostanie wyświetlona strona Swagger interfejsu użytkownika, kliknij pozycję **listy zadań** , aby wyświetlić dostępne metody.

    ![Swagger dostępne metody interfejsu użytkownika](./media/app-service-api-dotnet-get-started/methods.png)

9. Kliknij pierwszy przycisk **Pobierz** na liście.

10. W sekcji **Parametry** wprowadź gwiazdkę jako wartość `owner` parametr, a następnie kliknij przycisk **Spróbuj go**.

    Po dodaniu uwierzytelniania w samouczkach później warstwa środkowa zapewni rzeczywisty użytkownika do warstwy danych. Teraz wszystkie zadania będą mieć gwiazdki jako Identyfikatora właściciela aplikacji uruchomiony bez uwierzytelnianie jest włączone.

    ![Swagger interfejsu użytkownika przetestuj](./media/app-service-api-dotnet-get-started/gettryitout1.png)

    Interfejs użytkownika Swagger połączeń pobieranie listy zadań, metody i wyświetla kod odpowiedzi i JSON wyników.

    ![Swagger interfejsu użytkownika przetestuj wyników](./media/app-service-api-dotnet-get-started/gettryitout.png)

11. Kliknij **wpis**, a następnie kliknij pole w obszarze **Schematu modelu**.

    Kliknięcie w schemacie modelu prefills miejsce, w którym można określić wartości parametru metody wpis w polu wprowadzania danych. (Jeśli ta metoda nie działa w programie Internet Explorer, korzystania z innej przeglądarki lub ręczne wprowadzanie wartości parametru w następnym kroku).  

    ![Swagger interfejsu użytkownika przetestuj wpisu](./media/app-service-api-dotnet-get-started/post.png)

12. Zmienianie JSON w `todo` parametrów wejściowych pole tak, aby wyglądał jak w następującym przykładzie lub zastąpić tekst opis:

        {
          "ID": 2,
          "Description": "buy the dog a toy",
          "Owner": "*"
        }

13. Kliknij przycisk **Spróbuj go**.

    Interfejs API listy zadań zwraca kod odpowiedzi HTTP 204, który wskazuje sukcesu.

14. Kliknij pierwszy przycisk **Pobierz** , a następnie w tej sekcji strony kliknij przycisk **Spróbuj go** .

    Uzyskiwanie odpowiedzi metody zawiera teraz jesteś nowym użytkownikiem element.

15. Opcjonalnie: Wypróbuj także położenie, usuwanie i uzyskać za pomocą metody identyfikator.

16. Zamknij przeglądarkę i Zatrzymaj debugowanie programu Visual Studio.

Swashbuckle działa z projektu interfejs API sieci Web programu ASP.NET. Jeśli chcesz dodać Generowanie metadanych Swagger w istniejącym projekcie, wystarczy zainstalować pakiet Swashbuckle.

>[AZURE.NOTE] Metadane swagger zawiera unikatowy identyfikator każdej operacji interfejsu API. Domyślnie Swashbuckle może generować zduplikowane Swagger obsługę identyfikatorów Twoje metody kontroler interfejs API sieci Web. Tak się dzieje, gdy kontrolerze występują nadmiernie metod HTTP, takich jak `Get()` i `Get(id)`. Aby uzyskać informacje dotyczące obsługi overloads, zobacz [Definicje wygenerowane Dostosowywanie Swashbuckle interfejsu API](app-service-api-dotnet-swashbuckle-customize.md). Po utworzeniu projektu interfejs API sieci Web w programie Visual Studio przy użyciu szablonu aplikacji interfejsu API Azure kod generowany operacji unikatowych identyfikatorów zostaną automatycznie dodane do pliku *SwaggerConfig.cs* .  

## <a id="createapiapp"></a>Tworzenie aplikacji programu interfejsu API platformy Azure i wdrażanie kodu

W tej sekcji można użyć narzędzia Azure, które zostały zintegrowane w Kreatorze Visual Studio **Publikowanie sieci Web** do utworzenia nowej aplikacji interfejsu API platformy Azure. Następnie wdrożenie projektu ToDoListDataAPI do nowej aplikacji interfejsu API i połączeń, uruchamiając Swagger interfejs API.

1. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy ToDoListDataAPI projektu, a następnie kliknij **Publikuj**.

    ![Kliknij przycisk Publikuj w programie Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu.png)

2.  W kroku **profilu** kreatora **Publikowanie sieci Web** kliknij pozycję **Microsoft Azure aplikacji usługi**.

    ![Kliknij pozycję Azure publikowanie aplikacji usługi w sieci Web](./media/app-service-api-dotnet-get-started/selectappservice.png)

3. Zaloguj się do swojego konta Azure, jeśli nie masz już gotowe lub Odśwież poświadczenia, jeśli są one wygasła.

4. W oknie dialogowym aplikacji usługi wybierz Azure **subskrypcji** , którego chcesz użyć, a następnie kliknij przycisk **Nowy**.

    ![Kliknij przycisk Nowy w oknie dialogowym aplikacji usługi](./media/app-service-api-dotnet-get-started/clicknew.png)

    Zostanie wyświetlona karta **macierzyste** okna dialogowego **Tworzenie aplikacji usługi** .

    Ponieważ jest instalowany projekt interfejs API sieci Web, który ma zainstalowany Swashbuckle, Visual Studio założono, że utworzyć aplikację programu interfejsu API. To jest wskazywany według tytułu **Nazwa aplikacji interfejsu API** i fakt, że na liście rozwijanej **Zmień typ** jest ustawiona na **Interfejsu API aplikacji**.

    ![Typ aplikacji w oknie dialogowym aplikacji usługi](./media/app-service-api-dotnet-get-started/apptype.png)

5. Wprowadź **Nazwę aplikacji interfejsu API** , która jest unikatowa w domenie *azurewebsites.net* . Możesz zaakceptować domyślną nazwę, której proponuje Visual Studio.

    Jeśli zostanie wprowadzona nazwa, która już została użyta innej osobie, pojawi się czerwony wykrzyknik w prawo.

    Adres URL aplikacji interfejsu API będzie `{API app name}.azurewebsites.net`.

6. **Grupa zasobów** listy rozwijanej kliknij pozycję **Nowy**, a następnie wprowadź "ToDoListGroup" lub inna nazwa, jeśli wolisz.

    Grupa zasobów to zbiór Azure zasobów, takich jak aplikacje interfejsu API, baz danych, maszyny wirtualne i tak dalej. Ten samouczek najlepiej utworzyć nową grupę zasobów, ponieważ ułatwia do usunięcia w jednym kroku wszystkie zasoby Azure tworzonych samouczka.

    To pole umożliwia wybieranie istniejącej [grupy zasobów](../azure-resource-manager/resource-group-overview.md) lub utworzyć nową, wpisując nazwę różni się od dowolnego istniejącej grupy zasobów w ramach subskrypcji.

7. Kliknij przycisk **Nowy** **Plan usługi aplikacji** listy rozwijanej.

    Zrzut ekranu zawiera wartości próbki dla **Interfejsu API nazwę aplikacji**, **subskrypcji**i **Grupa zasobów** — wartości będą inne.

    ![Tworzenie aplikacji usługi okno dialogowe](./media/app-service-api-dotnet-get-started/createas.png)

    W następującej procedurze możesz utworzyć plan aplikacji usług dla nowej grupy zasobów. Plan usług aplikacji określa zasoby obliczeń, które aplikacji interfejsu API działa na. Na przykład jeśli wybierzesz warstwie bezpłatne, aplikacji interfejsu API jest uruchamiany w udostępnionej maszyny wirtualne, dla niektórych poziomów płatnej uruchomiony w dedykowanej maszyny wirtualne. Aby uzyskać informacje o planach usługi aplikacji zobacz [Omówienie plany aplikacji usługi](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

8. W oknie dialogowym **Konfigurowanie Plan usługi aplikacji** Jeśli wolisz wprowadź "ToDoListPlan" lub inna nazwa.

9. Na liście rozwijanej **lokalizacji** wybierz lokalizację, zbliżony do Ciebie.

    To ustawienie określa, które Azure centrum danych aplikacji będzie działać w. Wybierz lokalizację zbliżony do Minimalizuj [czas oczekiwania](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).

10. W **rozmiar** listy rozwijanej kliknij pozycję **Free**.

    Ten samouczek warstwie cennik bezpłatne zapewni wydajność wystarczającą.

11. W oknie dialogowym **Konfigurowanie Plan usługi aplikacji** kliknij **przycisk OK**.

    ![Kliknij przycisk OK w Konfigurowanie Plan usług aplikacji](./media/app-service-api-dotnet-get-started/configasp.png)

12. W oknie dialogowym **Tworzenie aplikacji usługi** kliknij przycisk **Utwórz**.

    ![Kliknij przycisk Utwórz w oknie dialogowym Tworzenie aplikacji usługi](./media/app-service-api-dotnet-get-started/clickcreate.png)

    Program Visual Studio tworzy aplikacja interfejsu API i profil publikowania, który zawiera wszystkie wymagane ustawienia aplikacji interfejsu API. Następnie otwiera kreatora **Publikowanie sieci Web** , w którym będą używane do wdrożenia projektu.

    Na karcie **połączenia** (jak pokazano poniżej) zostanie otwarty Kreator **Publikowania w sieci Web** .

    Na karcie **połączenie** ustawienia **serwera** i **Nazwa witryny** wskaż aplikacji interfejsu API. **Nazwa użytkownika** i **hasło** są poświadczenia wdrożenia, które tworzy Azure. Po wdrożeniu Visual Studio spowoduje otwarcie przeglądarki **Docelowy adres URL** (który jest wyłącznie w celu dla **Docelowy adres URL**).  

13. Kliknij przycisk **Dalej**.

    ![Kliknij przycisk Dalej na karcie połączenia publikowanie sieci Web](./media/app-service-api-dotnet-get-started/connnext.png)

    Następnej karty jest karta **Ustawienia** (jak pokazano poniżej). W tym miejscu możesz zmienić kartę Konfiguracja kompilacji wdrożenia Kompilacja debugowania [zdalne](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)debugowanie. Karcie również oferuje kilka **Opcji publikowanie plików**:

    * Usuwanie plików dodatkowe miejsce docelowe
    * Wstępnej kompilacji podczas publikowania
    * Wykluczanie plików w folderze App_Data

    Ten samouczek nie potrzebujesz żadnych z tych. Aby uzyskać szczegółowe informacje o co zrobić, zobacz [jak: Wdrażanie sieci Web za pomocą jednego kliknięcia publikowanie projektu w programie Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).

14. Kliknij przycisk **Dalej**.

    ![Kliknij przycisk Dalej na karcie Ustawienia publikowania w sieci Web](./media/app-service-api-dotnet-get-started/settingsnext.png)

    Następny jest kartę **Podgląd** (jak pokazano poniżej), która zapewnia szansy sprzedaży, aby sprawdzić, jakie pliki będą do skopiowania z programu project do aplikacji interfejsu API. Gdy projektu jest instalowany do aplikacji interfejsu API, które możesz już wdrożony w starszym, zostaną skopiowane tylko zmienionych plików. Jeśli chcesz wyświetlić listę, które mają zostać skopiowane, kliknięcie przycisku **Uruchom Podgląd** .

15. Kliknij przycisk **Publikuj**.

    ![Kliknij pozycję Publikuj w Podgląd na karcie publikowanie w sieci Web](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Program Visual Studio wdraża projektu ToDoListDataAPI w nowej aplikacji interfejsu API. Okno **dane wyjściowe** dzienniki pomyślnego wdrożenia, a zostanie wyświetlona strona "pomyślnie utworzona" w oknie przeglądarki do określonego adresu URL aplikacji interfejsu API.

    ![Dane wyjściowe okna pomyślnego wdrożenia](./media/app-service-api-dotnet-get-started/deploymentoutput.png)

    ![Nowa strona aplikacja utworzona pomyślnie interfejsu API](./media/app-service-api-dotnet-get-started/appcreated.png)

16. Dodawanie "swagger" do adresu URL na pasku adresu przeglądarki, a następnie naciśnij klawisz Enter. (Adres URL jest `http://{apiappname}.azurewebsites.net/swagger`.)

    Przeglądarka wyświetla samej Swagger interfejsu użytkownika, który wcześniej pokazano, ale teraz jest uruchomiony w chmurze. Wypróbuj metody Get, a zobaczysz, że jesteś powrót do wykonania 2 domyślne. Wcześniej wprowadzone zmiany zostały zapisane w pamięci na komputerze lokalnym.

17. Otwórz [Azure portal](https://portal.azure.com/).

    Azure portal jest interfejs sieci web do zarządzania Azure zasobów, takich jak aplikacje interfejsu API.

18. Kliknij pozycję **więcej usług > usług aplikacji**.

    ![Przeglądanie usług aplikacji](./media/app-service-api-dotnet-get-started/browseas.png)

19. Karta **Aplikacji usług** Znajdź i kliknij przycisk z nowej aplikacji interfejsu API. (W portalu usługi Azure systemu windows, które otwierają w prawo są nazywane *karty*).

    ![Karta usług aplikacji](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

    Karty dwóch Otwórz. Karta jeden ma Omówienie aplikacji interfejsu API i będzie miał o dużej liczbie ustawień, które mogą przeglądać i zmieniać.

20. Karta **Ustawienia** Znajdź sekcję **interfejsu API** i kliknij przycisk **Definicji interfejsu API**.

    ![Interfejs API definicji karta Ustawienia](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    Karta **Definicji interfejsu API** pozwala określić adres URL, który zwraca metadanych Swagger 2.0 w formacie JSON. Gdy Visual Studio tworzy aplikacji interfejsu API, adres URL definicji interfejsu API ustawia na wartość domyślną dla wygenerowane Swashbuckle metadane pokazano wcześniej, czyli aplikacji interfejsu API jest podstawowy adres URL plus `/swagger/docs/v1`.

    ![Adres URL definicji interfejsu API](./media/app-service-api-dotnet-get-started/apidefurl.png)

    Po wybraniu aplikacji interfejsu API do wygenerowania kodu klienta dla niego Visual Studio pobiera metadane z tego adresu URL.

## <a id="codegen"></a>Generowanie kodu klienta dla warstwy danych

Jedną z zalet korzystania z integracji Swagger aplikacje Azure API jest generowanie kodu automatyczne. Klienta wygenerowany zajęcia ułatwiają wpisz kod, który wymaga aplikacji interfejsu API.

Projekt ToDoListAPI już ma kod wygenerowane klienta, ale w następującej procedurze będzie ją usunąć i ponownie wygenerować go, aby zobaczyć jak wykonać generowanie kodu.

1. W Visual Studio **Eksplorator rozwiązań**w programie project ToDoListAPI Usuń *ToDoListDataAPI* folder. **Uwaga: Usuwanie tylko folder, a nie projektu ToDoListDataAPI.**

    ![Usuń wygenerowany kod klienta](./media/app-service-api-dotnet-get-started/deletecodegen.png)

    Ten folder został utworzony przy użyciu procesu tworzenia kod, którego chcesz przejść.

2. Kliknij prawym przyciskiem myszy projektu ToDoListAPI, a następnie kliknij pozycję **Dodaj > klienta API pozostałych**.

    ![Dodawanie klienta interfejsu API usługi REST w programie Visual Studio](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3. W oknie dialogowym **Dodawanie klienta API pozostałych** kliknij **Swagger adresu URL**, a następnie kliknij **Wybierz trwałego Azure**.

    ![Wybierz trwały Azure](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

4. W oknie dialogowym **Aplikacji usługi** Rozwiń grupy zasobów używanej do tego samouczka i wybierz aplikację interfejsu API, a następnie kliknij **przycisk OK**.

    ![Wybierz aplikację interfejsu API do generowania kodu](./media/app-service-api-dotnet-get-started/codegenselect.png)

    Zwróć uwagę, że po powrocie do okna dialogowego **Dodawanie klienta API pozostałe** pola tekstowego został wprowadzony przy użyciu definicji interfejsu API wartość adresu URL, którego pokazano wcześniej w portalu.

    ![Adres URL definicji interfejsu API](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    >[AZURE.TIP] Alternatywny sposobem uzyskania metadanych do generowania kodu jest wprowadź adres URL bezpośrednio zamiast przechodzące przez okna dialogowego przeglądania. Lub jeśli chcesz wygenerować kod klienta przed wdrożeniem Azure, może uruchomić lokalnie projekt interfejs API sieci Web, przejdź do adresu URL zawierającego plik Swagger JSON, Zapisz plik, a następnie użyj opcji **Wybierz istniejący plik metadanych Swagger** .

5. W oknie dialogowym **Dodawanie pozostałych interfejsu API klienta** kliknij **przycisk OK**.

    Program Visual Studio tworzy folder o nazwie po aplikacji interfejsu API i generuje klasy klienta.

    ![Pliki kodu wygenerowane klienta](./media/app-service-api-dotnet-get-started/codegenfiles.png)

6. W programie project ToDoListAPI Otwórz *Controllers\ToDoListController.cs* , aby wyświetlić kod w wierszu 40 połączeń API przy użyciu wygenerowanych klienta.

    Poniższy fragment pokazano, jak kod tworzy wystąpienie obiektu klienta i wywołuje metodę Get.

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }

        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }

    Parametr konstruktora otrzymuje adres URL punktu końcowego z `toDoListDataAPIURL` ustawienia aplikacji. W pliku Web.config tej wartości ustawiono do lokalnego programu IIS Express adresu URL projektu interfejsu API tak, aby uruchomić aplikację lokalnie. Jeśli parametr konstruktora domyślny punkt końcowy jest adres URL, który został wygenerowany z kodu.

7. Klasy klient zostanie wygenerowany pod inną nazwą według nazwy aplikacji interfejsu API; Zmień kod w *Controllers\ToDoListController.cs* , aby odpowiadały Nazwa typu co został wygenerowany w projekcie. Na przykład jeśli nazwę swojej ToDoListDataAPI071316 aplikacji interfejsu API, możesz zmienić kod:

        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

w tym:

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-to-host-the-middle-tier"></a>Tworzenie aplikacji programu interfejsu API do obsługi inicjał drugiego poziomu

Wcześniej zostanie [utworzona aplikacji interfejsu API Warstwa danych i używany kod, aby go](#createapiapp).  Teraz możesz postępuj zgodnie z procedurą dla aplikacji interfejsu API inicjał drugiego poziomu.

1. W **Eksploratorze rozwiązań**, kliknij prawym przyciskiem myszy warstwa środkowa ToDoListAPI projektu (nie warstwy danych ToDoListDataAPI), a następnie kliknij pozycję **Publikuj**.

    ![Kliknij przycisk Publikuj w programie Visual Studio](./media/app-service-api-dotnet-get-started/pubinmenu2.png)

2.  Na karcie **profil** kreatora **Publikowania w sieci Web** kliknij pozycję **Microsoft Azure aplikacji usługi**.

3. W oknie dialogowym **Aplikacji usługi** kliknij przycisk **Nowy**.

4. Na karcie **macierzyste** w oknie dialogowym **Tworzenie aplikacji usługi** zaakceptować domyślną **Nazwę aplikacji interfejsu API** lub wprowadź nazwę, która jest unikatowa w domenie *azurewebsites.net* .

5. Wybierz pozycję Azure **subskrypcji** był używany.

6. W rozwijane **Grupa zasobów** wybierz tej samej grupy zasobów, utworzony wcześniej.

7. W **Aplikacji Plan usług** listę rozwijaną wybierz tego samego planu, który został utworzony wcześniej. Będzie on domyślnej do tej wartości.

8. Kliknij przycisk **Utwórz**.

    Programu Visual Studio tworzy aplikacji interfejsu API, tworzy profilu publikowania i wyświetla **połączenia** — krok kreatora **Publikowania w sieci Web** .

9.  W kroku **połączenia** w kreatorze **Publikowanie sieci Web** kliknij pozycję **Publikuj**.

    Program Visual Studio wdraża projektu ToDoListAPI w nowej aplikacji interfejsu API i otwarcie przeglądarki do określonego adresu URL aplikacji interfejsu API. Zostanie wyświetlona strona "utworzono".

## <a name="configure-the-middle-tier-to-call-the-data-tier"></a>Konfigurowanie warstwa środkowa połączenie warstwy danych

Jeśli aplikacja warstwa środkowa interfejsu API teraz, czy spróbuj nawiązać połączenie przy użyciu adresu URL hosta lokalnego, który jeszcze znajduje się w pliku Web.config warstwy danych. W tej sekcji, należy wprowadzić adres URL aplikacji danych Warstwa interfejsu API w ustawienie środowiska w aplikacji interfejsu API warstwy środkowej. Kod w aplikacji warstwa środkowa interfejsu API pobiera ustawienie adresu URL Warstwa danych, ustawienia środowiska zastępują co znajduje się w pliku Web.config.

1. Przejdź do [portalu Azure](https://portal.azure.com/), a następnie przejdź do karta **Interfejsu API aplikacji** dla aplikacji interfejsu API utworzone przez Ciebie do obsługi projektu TodoListAPI (inicjał drugiego poziomu).

2. W aplikacji interfejsu API karta **Ustawienia** kliknij pozycję **Ustawienia aplikacji**.

3. W aplikacji interfejsu API karta **Ustawienia aplikacji** przewiń w dół do sekcji **Ustawienia aplikacji** i Dodaj następujący klucz i wartość. Wartość będzie adres URL aplikacji do pierwszego interfejsu API opublikowanych w tym samouczku.

  	| **Klawisz** | toDoListDataAPIURL |
  	|---|---|
  	| **Wartość** | Nazwa aplikacji Warstwa interfejsu API danych HTTPS://{your} .azurewebsites .net |
  	| **Przykład** | https://todolistdataapi.azurewebsites.NET |

4. Kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz ustawienia aplikacji](./media/app-service-api-dotnet-get-started/asinportal.png)

    Po uruchomieniu kodu platformy Azure tę wartość zastąpi teraz host lokalny adres URL, który znajduje się w pliku Web.config.

## <a name="test"></a>Test

1. W oknie przeglądarki przejdź do adresu URL nowej aplikacji interfejsu API warstwy środkowej, która została właśnie utworzona dla ToDoListAPI. Możesz uzyskać, klikając adres URL w aplikacji interfejsu API karta głównym w portalu.

2. Dodawanie "swagger" do adresu URL na pasku adresu przeglądarki, a następnie naciśnij klawisz Enter. (Adres URL jest `http://{apiappname}.azurewebsites.net/swagger`.)

    Przeglądarka wyświetla samej Swagger interfejsu użytkownika, który został wyświetlony wcześniej dla ToDoListDataAPI, ale teraz `owner` nie jest wymagane pola dla operacji Get, ponieważ aplikacji warstwa środkowa interfejsu API wysyła tę wartość do aplikacji interfejsu API Warstwa danych dla Ciebie. (Po wykonaniu samouczki uwierzytelniania warstwa środkowa wyśle rzeczywiste identyfikatory `owner` parametru; dla teraz go jest słabo kodowania gwiazdkę.)

3. Wypróbuj metody Get i innych metod do sprawdzania poprawności aplikacji interfejsu API warstwa środkowa pomyślnie dzwoni aplikacji interfejsu API Warstwa danych.

    ![Swagger metody Get interfejsu użytkownika](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

W przypadku, gdy napotkasz problem podczas wykonywania kroków poniżej tego samouczka jest następujących wskazówek dotyczących rozwiązywania problemów:

* Upewnij się, że korzystasz z najnowszą wersją pakietu [SDK Azure dla środowiska .NET](http://go.microsoft.com/fwlink/?linkid=518003).

* Dwa nazw projektów są podobne (ToDoListAPI, ToDoListDataAPI). Jeśli elementy nie wyglądają zgodnie z opisem w instrukcjach podczas pracy z projektem, upewnij się, że został otwarty poprawny projektu.

* Jeśli jesteś w sieci firmowej i próbujesz wdrożyć usługę aplikacji Azure przez zaporę, upewnij się, że porty 443 i 8172 są otwarte dla wdrażanie sieci Web. Jeśli nie możesz otworzyć te porty, używając inne metody wdrażania.  Zobacz [Wdrażanie aplikacji usłudze Azure aplikacji](../app-service-web/web-sites-deploy.md).

* Błędy "nazw trasa musi być unikatowa" — można uzyskać te Jeśli przypadkowo wdrażanie aplikacji interfejsu API problem projektu i następnie wdrożyć poprawne jeden do niego. Aby rozwiązać ten problem, ponownego wdrażania poprawne projektu do aplikacji interfejsu API i na karcie **Ustawienia** w kreatorze **Publikowania w sieci Web** kliknij przycisk **Usuń pliki dodatkowe w miejscu docelowym**.

Po umieszczeniu aplikacji interfejsu API programu ASP.NET działa usługa Azure aplikacji, warto dowiedzieć się więcej na temat funkcji programu Visual Studio, które uproszczenia rozwiązywania problemów. Aby uzyskać informacje o rejestrowaniu zdalne debugowanie i uzyskać więcej informacji, zobacz [aplikacje usługi aplikacji Azure rozwiązywania problemów w programie Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).

## <a name="next-steps"></a>Następne kroki

Użytkownik zobaczył, jak wdrażać istniejących projektów interfejs API sieci Web do aplikacji interfejsu API, generowanie kodu klientów w przypadku aplikacji interfejsu API i używanie aplikacji interfejsu API z klientami .NET. Następnego samouczka, w tym zadaniu przedstawiono sposób [używania CORS do korzystania z aplikacji interfejsu API języka JavaScript klientów](app-service-api-cors-consume-javascript.md).

Aby uzyskać więcej informacji na temat generowanie kodu klienta Zobacz repozytorium [Azure i AutoRest](https://github.com/azure/autorest) na GitHub.com. Aby uzyskać pomoc dotyczącą problemów przy użyciu wygenerowanych klienta Otwórz [problem w repozytorium AutoRest](https://github.com/azure/autorest/issues).

Jeśli chcesz utworzyć nowy interfejs API aplikacji projektów od podstaw, użyj szablonu **Aplikacji interfejsu API Azure** .

![Szablon aplikacji interfejsu API programu Visual Studio](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

Szablon projektu **Azure interfejsu API aplikacji** jest równoważna wybieranie **pustego** ASP.NET 4.5.2 szablon, klikając pole wyboru, aby dodać obsługę interfejsu API sieci Web i instalacji pakietu Swashbuckle NuGet. Ponadto szablon dodaje niektórych kod konfiguracji Swashbuckle, mające na celu zapobieganie Tworzenie zduplikowanych operacji Swagger identyfikatorów. Po utworzeniu aplikacji interfejsu API programu project można wdrażać go do aplikacji interfejsu API taki sam sposób, jak pokazano w tym samouczku.
