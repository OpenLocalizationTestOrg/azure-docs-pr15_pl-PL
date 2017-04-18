<properties
    pageTitle="Wybieranie między przepływu, logiki aplikacji, funkcji i WebJobs | Microsoft Azure"
    description="Porównywanie i kontrast chmury Integracja usług firmy Microsoft i zdecydować, które należy używać usługi."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="Microsoft przepływu, przepływu, logiki aplikacji, funkcji azure, funkcje azure webjobs, webjobs, przetwarzanie, dynamiczne obliczeń pliki architektura zdarzenia"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Wybieranie między przepływu, logika aplikacji, funkcji i WebJobs

W tym artykule porównano i różnic między następujące usługi w chmurze firmy Microsoft, które można rozwiązać wszystkie problemy integracji i automatyzacji procesów biznesowych:

- [Przepływ firmy Microsoft](https://flow.microsoft.com/)
- [Aplikacje logiczny Azure](https://azure.microsoft.com/services/logic-apps/)
- [Funkcje Azure](https://azure.microsoft.com/services/functions/)
- [WebJobs Azure aplikacji usługi](../app-service-web/web-sites-create-web-jobs.md)

Wszystkie te usługi są przydatne, gdy "przyklejanie" razem odmienne systemy. Wszystkie definiować danych wejściowych, akcje, warunki i dane wyjściowe. Każdy z nich można uruchamiać na harmonogram lub wyzwalacza. Jednak każdej usługi dodaje unikatowego zestawu wartości i ich porównanie nie jest pytanie "która usługa jest najlepszy?" oprócz jednego "usługę najlepiej nadaje się do tej sytuacji?" Często kombinacji tych usług jest najlepszym sposobem na szybko utworzyć rozwiązanie skalowalna, pełny integracji polecane.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Przepływ a logiki aplikacji

Można omówimy Flow firmy Microsoft i aplikacje logiki Azure razem ponieważ są one obu usług integracji *pierwszej konfiguracji* , które ułatwia tworzenie procesów i przepływy pracy i integracja z różnych aplikacji władz akredytacji bezpieczeństwa i enterprise. 

- Przepływ jest tworzona u góry aplikacji warunków logicznych
- Mają samej Projektant przepływów pracy
- Praca w jednym obsługuje również w innych [łączników](../connectors/apis-list.md)

Przepływów uprawnia pracownika pakietu office do wykonywania prostych integracji (np. uzyskać SMS ważne wiadomości e-mail) bez pośrednictwa deweloperów lub IT. Z drugiej strony aplikacje logiczny można włączyć integracji zaawansowane lub krytyczne (np. B2B procesy), gdzie wymagane są rozwiązania DevOps i zabezpieczenia poziomie przedsiębiorstwa. Jest zazwyczaj firm przepływ pracy ma powiększyć złożoność pracy w nadgodzinach. W związku z tym można rozpocząć przepływu na początku, a następnie przekonwertuj ją na aplikację logiczny, stosownie do potrzeb.

Poniższa tabela ułatwia określanie, czy przepływu lub logicznych aplikacji jest najlepszy dla danego integracji.

|               | Przepływ                                                                             | Aplikacje warunków logicznych                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Grupy odbiorców      | Office pracowników, użytkowników biznesowych                                                   | Informatycy i deweloperów                                                                                 |
| Scenariusze     | Internetowy                                                                     | Krytyczne                                                                                    |
| Narzędzia do projektowania   | W przeglądarce, tylko interfejsu użytkownika                                                              | W przeglądarce i [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md), dostępne [widoku kodu](../app-service-logic/app-service-logic-author-definitions.md) |
| DevOps        | Ad hoc opracowywanie produkcji                                                    | Formant źródła, testowanie, pomocy technicznej i automatyzacji i łatwości zarządzania w [Zarządzania zasobami Azure](../app-service-logic/app-service-logic-arm-provision.md)|
| Środowiska pracy — administrator| [https://Flow.microsoft.com](https://flow.microsoft.com)                       | [https://Portal.Azure.com](https://portal.azure.com)                                                |
| Zabezpieczenia      | Standardowy wskazówki: [suwerenności danych](https://wikipedia.org/wiki/Technological_Sovereignty), [szyfrowanie w pozostałych](https://wikipedia.org/wiki/Data_at_rest#Encryption) za dane poufne itd. | Zapewnienie zabezpieczeń Azure: [Zabezpieczenia Azure](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Centrum zabezpieczeń](https://azure.microsoft.com/services/security-center/), [dzienników inspekcji](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)i innych elementów. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Funkcje a WebJobs

Firma Microsoft mogą dyskutować funkcji Azure i Azure aplikacji usługi WebJobs ze sobą, ponieważ są one usług integration services obu *pierwszego kodu* i przeznaczone dla deweloperów. Umożliwiają uruchamianie skryptu lub fragment kodu w odpowiedzi na różne zdarzenia, takich jak [Nowy magazyn obiektów blob](functions-bindings-storage.md) lub [WebHook żądanie](functions-bindings-http-webhook.md). Oto podobieństwa ich: 

- Zarówno są oparte na [Usłudze Azure aplikacji](../app-service/app-service-value-prop-what-is.md) i korzystać z funkcji, takich jak [kontrolki źródła](../app-service-web/app-service-continuous-deployment.md), [Uwierzytelnianie](../app-service/app-service-authentication-overview.md)i [monitorowania](../app-service-web/web-sites-monitor.md).
- Są oparte na Deweloper usług.
- Zarówno standardowy skryptów pomocy technicznej i językach programowania.
- Mają NuGet i NPM pomocy technicznej.

Funkcje jest naturalne kształtowanie WebJobs, ponieważ trwa najlepszych rzeczy, o WebJobs i zwiększa na nich. Ulepszenia obejmują: 

- Usprawniony deweloperów, testowanie i uruchom kodu bezpośrednio w przeglądarce.
- Wbudowane integrację z usług więcej Azure i 3rd firm usługi, takie jak [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Płatności użycia, bez konieczności [Aplikacji usługi Planowanie](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)opłatami.
- Automatyczne, [dynamiczne skalowania](functions-scale.md).
- W przypadku istniejących klientów usługi aplikacji, uruchomionych plan usług aplikacji nadal możliwe (Aby można było korzystać w obszarze wykorzystania zasobów).
- Integracja z aplikacji logicznych.

W poniższej tabeli podsumowano różnice między funkcjami i WebJobs:

|                        | Funkcje                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Skalowanie                | Configurationless skalowania                                                                                                                                                | Skalowanie z planem aplikacji usługi        |
| Cennik                | Płatności użycia lub jego części plan usług aplikacji                                                                                                                                  | Część planu aplikacji usługi           |
| Typ uruchomienia               | wyzwalane, według harmonogramu (przez wyzwalacz czasomierza)                                                                                                                                  | wyzwalane, ciągły, według harmonogramu   |
| Zdarzenia wyzwalacza         | [timer](functions-bindings-timer.md), [Azure DocumentDB](functions-bindings-documentdb.md), [Koncentratory zdarzenia Azure](functions-bindings-event-hubs), [HTTP/WebHook (GitHub, zapas czasu)](functions-bindings-http-webhook.md), [Mobile Azure aplikacji usługi](functions-bindings-mobile-apps.md), [Koncentratory powiadomienie Azure](functions-bindings-notification-hubs.md), [Bus usługi Azure](functions-bindings-service-bus.md), [Magazyn Azure](articles/functions-bindings-storage.md) | [Azure miejsca do magazynowania](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Bus usługi Azure](websites-dotnet-webjobs-sdk-service-bus.md)         |
| Rozwoju w przeglądarce | x                                                                                                                                                                        |                                    |
| Okno skryptów       | badawczych                                                                                                                                                             | x                                  |
| Programu PowerShell             | badawczych                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| Urodzinową                   | badawczych                                                                                                                                                             | x                                  |
| PHP                    | badawczych                                                                                                                                                             | x                                  |
| Python                 | badawczych                                                                                                                                                             | x                                  |
| Języka JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | badawczych                                                                                                                                                             | x                                  |

Czy chcesz używać funkcji lub WebJobs ostatecznie zależy od już wykonywanej za pomocą aplikacji usługi. Jeśli masz aplikację aplikacji usługi, dla którego chcesz uruchomić wstawki kodu, a chcesz zarządzać nimi w tym samym środowisku DevOps, należy użyć WebJobs. Jeśli chcesz uruchomić fragmenty kodu dla pozostałych usług Azure lub nawet 3rd firm aplikacje, lub jeśli chcesz zarządzać fragmenty kodu usługi integracji oddzielnie z aplikacji aplikacji usługi lub jeśli chcesz nawiązać połączenie z fragmenty kodu za pomocą aplikacji logiczny, możesz należy korzystać z wszystkie ulepszenia w funkcjach.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Zgrupowane, logiczny aplikacji i funkcji

Jak wcześniej wspomniano która usługa najlepiej nadaje się do Ciebie zależy od sytuacji. 

- Aby zoptymalizować firm proste a następnie użyj przepływu.
- Jeśli danego scenariusza integracji jest zbyt zaawansowane dla przepływu lub potrzebujesz możliwości DevOps i compliances zabezpieczeń, należy użyć aplikacji logiki.
- Jeśli krok programie integrację wymaga wysoce standardowe przekształcenie lub kod specjalistyczne, następnie napisać aplikacji dla funkcji, a następnie funkcja wyzwalacza jako akcji w aplikacji logicznych.

Można zadzwonić aplikacji logicznych w formie przepływu. Można również wywołać funkcji w aplikacji logiki i aplikacji logicznych w funkcji. Integracja między przepływu, logiki aplikacji i funkcji w dalszym ciągu usprawnić pracę w nadgodzinach. Można tworzyć czegoś w jedną usługę i używać go w innych usługach. Dlatego inwestycji, wszelkie wprowadzone w trzech technologii jest zastanowić.

## <a name="next-steps"></a>Następne kroki

Wprowadzenie wszystkich usług, tworząc do pierwszego przepływu, logiki aplikacji, funkcji aplikacji lub WebJob. Kliknij dowolną z poniższych łączy:

- [Wprowadzenie do programu Microsoft Flow](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Tworzenie aplikacji logiki](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Tworzenie pierwszego funkcja Azure](../azure-functions/functions-create-first-azure-function.md)
- [Wdrażanie WebJobs przy użyciu programu Visual Studio](../app-service-web/websites-dotnet-deploy-webjobs.md)

Możesz też uzyskać więcej informacji na temat tych usług integracji z poniższych łączy:

- [Używanie funkcji Azure i Azure aplikacji usługi dla scenariuszy integracji przez Christopher Anderson](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Uproszczone Lamanna Charlesa integracji](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Aplikacje logiki Live emisja](http://aka.ms/logicappslive)
- [Microsoft przepływ często zadawane pytania](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Zasoby dokumentacji WebJobs Azure](../app-service-web/websites-webjobs-resources.md)
