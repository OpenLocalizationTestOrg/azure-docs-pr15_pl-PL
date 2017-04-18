<properties
    pageTitle="Wdrożenie flighting (testowania wersji beta) w usłudze Azure aplikacji"
    description="Dowiedz się, jak lotu nowe funkcje w aplikacji lub wersji beta testowania aktualizacje w tym samouczku zakończenia do końca. Jego łączy funkcje aplikacji usługi, takie jak ciągły publikowania, gniazda routingu ruchu i integracji wniosków aplikacji."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/02/2016"
    ms.author="cephalin"/>
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a>Wdrożenie flighting (testowania wersji beta) w usłudze Azure aplikacji

Ten samouczek pokazano, jak wykonywać *flighting wdrożeń* za integracji różne możliwości [Azure aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=529714) i [Wniosków aplikacji Azure](/services/application-insights/). 

*Flighting* jest procesem wdrażania sprawdzania nowych funkcji i zmień z ograniczoną liczbą rzeczywistą klientów i testuje większych w środowisku produkcyjnym. Jest podobnie testowania wersji beta i jest nazywany "test kontroli lotów". Wiele dużych przedsiębiorstwach witrynę internetową Użyj tej metody, aby uzyskać najwcześniejsze sprawdzania poprawności ich aktualizacje aplikacji w praktyce ich [adaptacyjne](https://en.wikipedia.org/wiki/Agile_software_development)rozwoju. Usługa Azure aplikacji umożliwia integracja test produkcji z publikowaniem stałego i wniosków aplikacji w celu wykonania tego samego scenariusza DevOps. Zalety tej metody:

- **Wzmocnienia wydaniu aktualizacji _przed_ rzeczywistą opinii do produkcji** - grupowych jedynie lepiej niż uzyskania opinii zaraz po zwolnieniu jest uzyskiwanie opinii, przed zwolnieniem. Aktualizacje z rzeczywistego użytkownika ruchu i zachowania można sprawdzić, jak najwcześniej wymaganych cyklu życia produktu.
- **Ulepszanie [ciągły sterowanych test rozwoju (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development) ** – integrowanie test produkcji z ciągłym integracji i oprzyrządowania z wniosków aplikacji Weryfikacja użytkownika dzieje się wcześniej i automatycznie w cykl życia. W ten sposób zmniejszyć inwestycji czasu na wykonanie ręcznego testu.
- **Optymalizowanie testowania przepływu pracy** — dzięki automatyzacji test produkcji z ciągłym oprzyrządowania monitorowania, potencjalnie wykonasz cele różnego rodzaju badań w jednym procesie, takich jak [Integracja](https://en.wikipedia.org/wiki/Integration_testing) [regresji](https://en.wikipedia.org/wiki/Regression_testing), [użyteczności](https://en.wikipedia.org/wiki/Usability_testing), ułatwień dostępu, lokalizacji, [wydajności](https://en.wikipedia.org/wiki/Software_performance_testing), [zabezpieczeń](https://en.wikipedia.org/wiki/Security_testing)i [akceptacji](https://en.wikipedia.org/wiki/Acceptance_testing).

Wdrożenie flighting nie jest niemal routingu ruchu live. W takiej instalacji chcesz uzyskać wglądu tak szybko, jak to możliwe czy ona nieoczekiwany błąd, spadek wydajności, problemy środowiska użytkownika. Należy pamiętać, że ma się do czynienia z kontrahentami rzeczywistą. Aby to zrobić prawy, należy się upewnić, że skonfigurowano flighting wdrożenia zebrać wszystkie dane, które są potrzebne w celu podjęcia decyzji informacje do następnego kroku. W tym samouczku pokazano sposób zbierania danych za pomocą aplikacji wniosków, ale możesz użyć nowego Relic lub inne technologie, które pasuje do scenariusza. 

## <a name="what-you-will-do"></a>Co będzie zrobić

W tym samouczku dowiesz się, jak wyświetlić następujące scenariusze ze sobą, aby przetestować aplikacji aplikacji usługi produkcji:

- [Ruch produkcji trasy](app-service-web-test-in-production-get-start.md) do aplikacji w wersji beta
- [Dokument z aplikacji](../application-insights/app-insights-web-track-usage.md) uzyskanie przydatne metryki
- Stale wdrażanie aplikacji w wersji beta i śledzenie metryki live aplikacji
- Porównywanie metryk między app produkcji i aplikację beta, aby zobaczyć, jak zmiany kodu Przetłumacz na wyniki

## <a name="what-you-will-need"></a>Co będzie potrzebne

-   Konto Azure
-   Konto [GitHub](https://github.com/)
- Visual Studio 2015 - możesz pobrać [edition społeczności](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).
-   Powłoki cyfra (instalowana z [GitHub dla systemu Windows](https://windows.github.com/)) — pozwala na uruchomienie poleceń cyfra i programu PowerShell w tej samej sesji
-   Najnowsze bitów [Azure programu PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi)
-   Podstawy z następujących czynności:
    -   [Menedżer zasobów Azure](../azure-resource-manager/resource-group-overview.md) rozmieszczania szablonu (zobacz [Rozmieszczanie złożonej aplikacji właściwie platformy Azure](app-service-deploy-complex-application-predictably.md))
    -   [Cyfra](http://git-scm.com/documentation)
    -   [Programu PowerShell](https://technet.microsoft.com/library/bb978526.aspx)

> [AZURE.NOTE] Potrzebne jest konto Azure do użycia tego samouczka:
> + Możesz [otworzyć konto Azure bezpłatnie](/pricing/free-trial/) — pobranie środków Umożliwia wypróbowanie płatnych usług Azure, a nawet w przypadku, gdy są używane w górę możesz zachować konta i użyj wolny Azure usług, takich jak aplikacje sieci Web.
> + Można [aktywować korzyści subskrybentów Visual Studio](/pricing/member-offers/msdn-benefits-details/) — subskrypcji i Visual Studio umożliwia środków co miesiąc, używanej usługi Azure płatnej.
>
> Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](http://go.microsoft.com/fwlink/?LinkId=523751), którym natychmiast można utworzyć aplikację sieci web krótkotrwałe starter w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="set-up-your-production-web-app"></a>Konfigurowanie aplikacji sieci web produkcji

>[AZURE.NOTE] Skrypt używane w tym samouczku automatycznie skonfiguruje ciągły publikowania z repozytorium GitHub. Wymaga to, że poświadczenia GitHub znajdują się już w Azure, w przeciwnym razie rozmieszczenia z użyciem skryptów nie powiedzie się podczas próby skonfigurowania ustawień kontroli źródła dla aplikacji sieci web.
>
>Przechowywanie poświadczeń GitHub w Azure, tworzenie aplikacji sieci web w [Azure Portal](https://portal.azure.com/) i [skonfigurować wdrożenie GitHub](app-service-continuous-deployment.md#Step7). Wystarczy zrobić to tylko raz.

Typowy scenariusz DevOps masz aplikację, która jest uruchomiony żywo platformy Azure, a chcesz wprowadzić zmiany do publikowania ciągły. W tym scenariuszu produkcji będzie wdrożyć szablon, który ma opracowanych i sprawdzany.

1.  Tworzenie własnych rozwidlenie repozytorium [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) . Informacje na temat tworzenia usługi rozwidlenie zobacz [rozwidlenie Repo](https://help.github.com/articles/fork-a-repo/). Po utworzeniu usługi rozwidlenie widać go w przeglądarce.

    ![](./media/app-service-agile-software-development/production-1-private-repo.png)

2.  Otwórz sesję powłoki cyfra. Jeśli nie masz jeszcze powłoki cyfra, zainstaluj teraz [GitHub dla systemu Windows](https://windows.github.com/) .
3.  Tworzenie lokalnej klonowanie usługi rozwidlenia, wykonując następujące polecenie:

        git clone https://github.com/<your_fork>/ToDoApp.git

4.  Po umieszczeniu swojej lokalnej klonowanie, przejdź do * &lt;repository_root >*\ARMTemplates i uruchom deploy.ps1 skrypt sufiksem unikatowe, tak jak pokazano poniżej:

        .\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix>

4.  Po wyświetleniu monitu wpisz odpowiednią nazwę użytkownika i hasło dostępu do bazy danych. Należy pamiętać poświadczenia bazy danych, ponieważ musisz określić je ponownie podczas aktualizowania grupy zasobów.

    Powinien zostać wyświetlony obsługi administracyjnej postępu różnych Azure zasobów. Po zakończeniu wdrożenia skrypt uruchamianie aplikacji w przeglądarce i zapewniają przyjazny dźwiękowy.
    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)

6.  Ponownie podczas sesji powłoki cyfra Uruchom:

        .\swap –Name ToDoApp<your_suffix>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)

7.  Po zakończeniu działania skrypt, wróć do przejdź do adresu serwera sieci Web (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) do Zobacz aplikacja działa w produkcji.
5.  Zaloguj się do usługi [Azure Portal](https://portal.azure.com/) i zapoznaj się co to jest tworzona.

    Powinny być widoczne dwie aplikacje sieci web w tej samej grupy zasobów, z `Api` sufiks w polu Nazwa. Jeśli możesz obejrzeć widoku grupa zasobów, pojawi się także z bazą danych SQL i serwer, plan usług aplikacji i tymczasową gniazda dla aplikacji sieci web. Przeglądanie różnych zasobów i porównaj je z * &lt;repository_root >*\ARMTemplates\ProdAndStage.json aby zobaczyć, jak są skonfigurowane w szablonie.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

Możesz zainstalować aplikację produkcji.  Teraz Przyjrzyjmy Załóżmy otrzymaniu opinii, że użyteczności jest niska aplikacji. Możesz zdecydować zbadać. Zamierzasz instrumentu Twojej aplikacji, aby zapewnić sobie opinii.

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a>Badanie: Instrumentu aplikacji klienckich monitorowania i miar

5. Otwórz * &lt;repository_root >*\src\MultiChannelToDo.sln w programie Visual Studio.
6. Przywracanie wszystkich pakietów Nuget, klikając prawym przyciskiem myszy rozwiązanie > **Zarządzaj pakietów NuGet rozwiązania** > **przywrócić**.
6. Kliknij prawym przyciskiem myszy **MultiChannelToDo.Web** > **Dodawanie aplikacji telemetrycznego wniosków** > **Konfigurowanie ustawień** > Zmień grupę zasobu do ToDoApp*&lt;your_suffix >* > **Dodawanie wniosków aplikacji do projektu**.
7. Azure Portal otwórz karta dla zasobu wglądu aplikacji **MultiChannelToDo.Web** . Następnie w obszarze **kondycji aplikacji** kliknij pozycję **Dowiedz się, jak zebrać danych ładowania strony przeglądarki** > Skopiuj kod.
7. Dodaj skopiowany kod oprzyrządowania JS, aby * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml tuż przed zamknięciem `<heading>` znacznik. Powinien zawierać klucz unikatowy oprzyrządowania zasobu wglądu aplikacji.

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>

11. Wysyłanie zdarzenia niestandardowe do aplikacji wniosków myszy kliknięć, dodając poniższy kod w dół treści:

        <script>
            $(document.body).find("*").click(function(event) {

                appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
            });
        </script>

    Ten wstawkę kodu JavaScript wysyła zdarzenia niestandardowego do aplikacji wniosków za każdym razem, gdy użytkownik kliknie w dowolnym miejscu w aplikacji sieci web.

12. W powłoce cyfra Przekaż, a następnie przekazać zmiany do swojego rozwidlenia w GitHub. Następnie odczekaj klientom Odśwież przeglądarkę.

        git add -A :/
        git commit -m "add AI configuration for client app"
        git push origin master

6.  Zamiana zmiany wdrożonej aplikacji produkcji:

        .\swap –Name ToDoApp<your_suffix>

13. Przejdź do zasobu wniosków aplikacji, który skonfigurowano. Kliknij przycisk zdarzenia niestandardowe.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

    Jeśli nie widzisz metryki dla zdarzenia niestandardowe, poczekaj kilka minut i kliknij przycisk **Odśwież**.

Załóżmy, że pojawi się wykres, takich jak poniżej:

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

I siatka zdarzenia poniżej:

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

Według kodzie aplikacji ToDoApp zdarzenie **przycisku** odpowiada przycisk Prześlij, a zdarzenie **wprowadzania** odpowiada pole tekstowe. Pory co miały znaczenie. Jednak wygląda jest dobrym ilości kliknięcia i bardzo kilku kliknięć na elementów do wykonania (zdarzenia **LI** ).

Oparte na ten formularz możesz usługi hipotezy, że niektórzy użytkownicy są mylić, która część interfejsu użytkownika jest wyświetlane i to, że kursor jest styl do zaznaczonego tekstu, gdy znajduje się na liście elementów i ich ikony.

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

Może to być contrived przykład. Jednak zamierzasz wprowadzić ulepszono aplikacji, a następnie wykonaj flighting wdrożenia uzyskanie użyteczności opinie klientów live.

### <a name="instrument-your-server-app-for-monitoringmetrics"></a>Instrumentu aplikacji serwer monitorowania i miar
Jest to tangens, ponieważ scenariusz pokazano w tym samouczku tylko dotyczy aplikacji klienta. Dla kompletności będą konfigurowane tak aplikacji po stronie serwera.

6. Kliknij prawym przyciskiem myszy **MultiChannelToDo** > **Dodawanie aplikacji telemetrycznego wniosków** > **Konfigurowanie ustawień** > Zmień grupę zasobu do ToDoApp*&lt;your_suffix >* > **Dodawanie wniosków aplikacji do projektu**.
12. W powłoce cyfra Przekaż, a następnie przekazać zmiany do swojego rozwidlenia w GitHub. Następnie odczekaj klientom Odśwież przeglądarkę.

        git add -A :/
        git commit -m "add AI configuration for server app"
        git push origin master

6.  Zamiana zmiany wdrożonej aplikacji produkcji:

        .\swap –Name ToDoApp<your_suffix>

To wszystko!

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a>Badanie: Dodawanie znaczników specyficzne dla przedział do swojego metryki aplikacji klienta

W tej sekcji skonfigurujesz gniazda rozmieszczania wysyłanie telemetrycznego specyficzne dla przedział do tego samego zasobu wniosków aplikacji. W ten sposób można porównać dane telemetryczne między ruchu z różnych gniazd (wdrożenia środowiskach) z łatwo zobaczyć efekt wprowadzonych zmian aplikacji. W tym samym czasie można oddzielić ruch produkcji od pozostałej, dzięki czemu można kontynuować monitorowanie aplikacji produkcji stosownie do potrzeb.

Ponieważ jest zbieranie danych na zachowanie klienta, konieczne będzie [dodać inicjator telemetrycznego kodu JavaScript](../application-insights/app-insights-api-custom-events-metrics.md#js-initializer) w index.cshtml. Jeśli chcesz przetestować wydajność po stronie serwera, na przykład, możesz też wykonać podobnie w kodzie server (zobacz [Aplikacji wniosków API dla zdarzenia niestandardowe i wskaźniki](../application-insights/app-insights-api-custom-events-metrics.md).

1. Najpierw dodaj bewteen kod dwóch `//` komentarze poniżej w języku JavaScript zablokować tym zostanie dodany do `<heading>` znakowanie wcześniej.

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    Powoduje, że kod inicjator `appInsights` obiekt, aby dodać właściwości niestandardowej o nazwie `Environment` na każdym urządzeniu telemetrycznego wysyła.

2. Następnie dodaj tej właściwości niestandardowe [Ustawienie przedział](web-sites-staged-publishing.md#AboutConfiguration) dla aplikacji sieci web platformy Azure. Aby to zrobić, uruchom następujące polecenia w sesji powłoki cyfra.

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    Definiuje już Web.config w projekcie `environment` ustawienia aplikacji. To ustawienie, podczas testowania lokalnie w aplikacji usługi metryki będzie być oznakowanie przy `VS Debugger`. Jednak po naciśnięciu zmiany Azure Azure będzie Znajdowanie i używanie `environment` zamiast tego ustawienia w konfiguracji aplikacji sieci web app i usługi metryki będzie oznakowanie przy `Production`.

3. Przekaż i przekazać zmiany kod do swojego rozwidlenie na GitHub, a następnie poczekaj, aż użytkownikom korzystanie z nowej aplikacji (wymagana Odśwież przeglądarkę). Trwa około 15 minut nowej właściwości były wyświetlane w Twojej aplikacji wniosków `MultiChannelToDo.Web` zasobów.

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master

4. Teraz, przejdź ponownie do karta **zdarzenia niestandardowe** i odfiltrować metryki `Environment=Production`. Teraz można można wyświetlić wszystkie nowe zdarzenia niestandardowe w przedział produkcji ten filtr.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)

5. Kliknij przycisk **Ulubione** , aby zapisać bieżące ustawienia Eksploratora metryki podobną do **zdarzenia niestandardowe: produkcji**. Możesz łatwo przełączać się między ten widok i w widoku przedział wdrożenia później.

    > [AZURE.TIP] Bardziej zaawansowanych analiz warto rozważyć [integracji zasób wniosków aplikacji usługi Power BI](../application-insights/app-insights-export-power-bi.md).

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a>Dodawanie znaczników specyficzne dla przedział do swojego serwera metryki aplikacji
Ponownie dla kompletności będzie skonfigurowaniu aplikacji po stronie serwera. W przeciwieństwie do aplikacji klienckich, które jest działają w JavaScript znaczniki przedział specyficzne dla aplikacji serwera jest narzędzia z kodem .NET.

1. Otwórz * &lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs. Dodawanie bloku kodu, po prostu przed zamknięciem nawias klamrowy nazw.

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }

2. Poprawianie błędów rozpoznawania nazw, dodając `using` instrukcje poniżej na początku pliku:

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;

3. Dodaj poniższy kod do początku `Application_Start()` metody:

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());

3. Przekaż i przekazać zmiany kodu do swojego rozwidlenie na GitHub, a następnie zaczekaj użytkownikom korzystanie z nowej aplikacji (wymagana Odśwież przeglądarkę). Trwa około 15 minut nowej właściwości były wyświetlane w Twojej aplikacji wniosków `MultiChannelToDo` zasobów.

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a>Aktualizacja: Konfigurowanie usługi gałąź beta

2. Otwórz * &lt;repository_root >*\ARMTemplates\ProdAndStagetest.json i Znajdź `appsettings` zasobów (wyszukiwanie `"name": "appsettings"`). Istnieje 4 je w jednym dla każdego przedział. 

2. Dla każdej `appsettings` zasobów, dodawanie `"environment": "[parameters('slotName')]"` ustawienie aplikacji do końca `properties` tablicy. Należy pamiętać zakończyć poprzedniego wiersza przecinkami.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)
    
    Właśnie dodanego `environment` ustawienie aplikacji do wszystkich gniazd w szablonie.
    
2. W tym samym pliku, Znajdź `slotconfignames` zasobów (wyszukiwanie `"name": "slotconfignames"`). Istnieją 2 je dla każdej aplikacji.

2. Dla każdej `slotconfignames` zasobów, dodawanie `"environment"` do końca `appSettingNames` tablicy. Należy pamiętać zakończyć poprzedniego wiersza przecinkami.

    Zostały wprowadzone `environment` aplikacji ustawienie USB na jego przedział wdrożenia odpowiednich dla obu aplikacji.  

3. W sesji powłoki cyfra, uruchom następujące polecenia z tym samym sufiksem grupa zasobów, używany przed.

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta

4. Po wyświetleniu monitu określ tych samych SQL bazy danych poświadczeń jako przed. Następnie, gdy zostanie wyświetlone pytanie, aby zaktualizować grupa zasobów, wpisz `Y`, następnie `ENTER`.

    Po zakończeniu skrypt, wszystkie zasoby w grupie zasobów oryginalny są zachowywane, ale nowy przedział o nazwie "beta" jest tworzona w nim w takiej samej konfiguracji jako przedział "Tymczasowego", który został utworzony na początku.

    >[AZURE.NOTE] Tej metody wdrażania różnych środowiskach tworzenia różni się od metody w [tworzeniu adaptacyjne oprogramowania w usłudze Azure aplikacji](app-service-agile-software-development.md). Tutaj możesz tworzyć środowiska wdrażania z gniazda wdrożenia, w którym ile tworzysz środowisk wdrażania z grup zasobów. Zarządzanie środowisk wdrażania przy użyciu grup zasobów pozwala na przechowywanie środowisku produkcyjnym off-limits deweloperów, ale nie jest proste testowanie produkcji, co można zrobić łatwo z gniazdami.

Jeśli chcesz, można również utworzyć aplikację alfa, uruchamiając

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

Ten samouczek możesz po prostu nadal używać aplikacji w wersji beta.

## <a name="update-push-your-updates-to-the-beta-app"></a>Aktualizacja: Push aktualizacje do aplikacji w wersji beta

Powrót do Twojej aplikacji, którą chcesz poprawić.

1. Upewnij się, że jest się teraz w swojej gałąź beta

        git checkout beta

2. W * &lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, Znajdź `<li>` znakowanie i dodawanie `style="cursor:pointer"` atrybutów, tak jak pokazano poniżej.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)

3. przekazywanie i wypychanych Azure.

4. Sprawdź, czy zmiany teraz są widoczne w przedział beta, przechodząc do http://todoapp*&lt;your_suffix >*-beta.azurewebsites.net/. Jeśli zmiana nie jest jeszcze widoczny, Odśwież przeglądarkę, aby uzyskać nowy kod javascript.

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

Teraz, gdy masz uruchomione w wersji beta przedział zmiany, możesz przystąpić do wykonywania flighting wdrożenia.

## <a name="validate-route-traffic-to-the-beta-app"></a>Sprawdzanie poprawności: Ruch trasy do aplikacji w wersji beta

W tej sekcji będą skierować ruch do aplikacji w wersji beta. For sake of jasności pokaz możesz zacząć rozsyłanie znaczną część ruch użytkownika do niego. W rzeczywistości natężenia ruchu, który chcesz skierować zależy od konkretnej sytuacji. Na przykład jeśli witryna znajduje się w skali microsoft.com, może wymagać mniej niż jeden procent sumy ruchu w celu uzyskania użytecznych danych.

1. W sesji powłoki cyfra, uruchom następujące polecenia do kierowania połowa ruchu produkcji do przedziału beta:

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

  `ReroutePercentage=50` Właściwość określa, że 50% ruchu produkcji będą kierowane do adresu URL aplikacji w wersji beta (określony przez `ActionHostName` właściwości).

2. Teraz przejść do http://ToDoApp*&lt;your_suffix >*. azurewebsites.net. 50% ruchu powinna teraz nastąpi przekierowanie do przedział beta.

3. W aplikacji wniosków zasób, filtrować metryki środowisko = "beta".

    > [AZURE.NOTE] Jeśli zapiszesz ten widok filtrowany jako ulubionego innego można łatwo Przerzuć widoki metryczne Eksploratora między produkcji i widoków beta.

Załóżmy, że w aplikacji wniosków pojawi się Strona podobna do następującej:

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

Nie jest to wyświetlane tylko czy wiele łatwy w `<li>` znaczniki, ale wydaje się wzrost kliknięć na `<li>` znaczniki. Następnie stwierdzić, że osoby odkryć nowy `<li>` znaczniki są wyświetlane i są teraz wyczyścić wszystkie ich wcześniej wykonane zadania w aplikacji.

Oparte na danych flighting wdrożenia, możesz określić, czy nowy interfejs użytkownika BYŁ jest gotowa do produkcji.

## <a name="go-live-move-your-new-code-into-production"></a>Przejdź na żywo: przenoszenie nowy kod produkcji

Teraz możesz przeprowadzić aktualizację produkcji. Co to jest doskonałe jest, że teraz już wiesz, że aktualizacja została już sprawdzonych _przed_ jego zostanie przypisany produkcji. Aby teraz można bezproblemowe wdrożyć go. Ponieważ wprowadzono aktualizację do aplikacji klienckich AngularJS sprawdzana poprawność tylko kod po stronie klienta. W przypadku zmiany do aplikacji sieci Web interfejsu API wewnętrznej, podobnie i łatwo można sprawdzić poprawności zmiany.

1. W powłoce cyfra usunąć reguły rozsyłania ruchu, uruchamiając następujące polecenie:

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()

2. Uruchom polecenia cyfra:

        git checkout master
        git pull origin master
        git merge beta
        git push origin master

2. Poczekaj kilka minut na nowy kod do wdrażania na tymczasowy przedział, a następnie uruchom http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net, aby zweryfikować, że nowa aktualizacja jest rozgrzane w tymczasowej przedział. Należy pamiętać, że gałąź wzorca do rozwidlenie jest połączony z tymczasowy przedział aplikacji.

3. Teraz Zamień tymczasowy przedział do produkcji

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a>Podsumowanie ##

Azure aplikacji usługi ułatwia dla małych — do średnich firm w celu przetestowania ich aplikacje klienta skierowaną w produkcji, coś zazwyczaj wykonane w dużych przedsiębiorstwach. Hopefully ten samouczek udzielił Ci wiedzy potrzebnych do umożliwiają powiązanie aplikacji usługi i wniosków aplikacji, aby flighting wdrażania, a nawet innych sytuacjach test w produkcji w Twój świat DevOps. 

## <a name="more-resources"></a>Więcej zasobów ##

-   [Adaptacyjne programowania Azure aplikacji usługi](app-service-agile-software-development.md)
-   [Konfigurowanie tymczasowej środowiska dla aplikacji sieci web w usłudze Azure aplikacji](web-sites-staged-publishing.md)
-   [Wdrażanie złożonej aplikacji właściwie platformy Azure](app-service-deploy-complex-application-predictably.md)
-   [Tworzenie szablonów Azure Menedżera zasobów](../resource-group-authoring-templates.md)
-   [JSONLint - JSON moduł sprawdzania poprawności](http://jsonlint.com/)
-   [Cyfra gałęzi — podstawowe gałęzi i scalanie](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
-   [Azure programu PowerShell](../powershell-install-configure.md)
-   [Projekt Kudu stron typu Wiki](https://github.com/projectkudu/kudu/wiki)
