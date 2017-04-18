<properties
    pageTitle="Usługa aplikacji interfejsu API aplikacje — informacje o zmianach | Microsoft Azure"
    description="Dowiedz się, co nowego interfejsu API aplikacji w usłudze Azure aplikacji."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>Usługa aplikacji interfejsu API aplikacje — informacje o zmianach

W zdarzeniu Connect() w 2015 listopada szereg ulepszeń usługi Azure aplikacji usługi były [ogłaszane](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Te udoskonalenia obejmują źródłowych zmiany interfejsu API aplikacje, aby lepiej wyrównać komórkowym i aplikacji Web Apps, zmniejszenie liczby koncepcja i poprawić wydajność środowisko uruchomieniowe i wdrażania. Rozpoczynanie nowych aplikacji interfejsu API 30 listopada 2015 r., możesz utworzyć za pomocą portalu zarządzania Azure lub najnowsze narzędzia zostaną zastosowane te zmiany. W tym artykule opisano te zmiany, a także jak ponownie rozmieścić istniejące aplikacje, aby korzystać z funkcji.

## <a name="feature-changes"></a>Zmiany funkcji
Kluczowe funkcje aplikacji interfejsu API — uwierzytelnianie, CORS i interfejsu API metadanych — przeniesiono bezpośrednio w aplikacji usługi. W przypadku tej zmiany funkcje są dostępne w sieci Web, Mobile i interfejsu API aplikacji. W rzeczywistości wszystkie trzy udostępnianie tego samego typu zasobu **Microsoft.Web/sites** w Menedżerze zasobów. Brama aplikacje interfejsu API jest już potrzebne lub oferowane z aplikacjami interfejsu API. Również ułatwia używać zarządzania interfejsu API Azure, ponieważ będą tylko jednej bramy zarządzania interfejsu API.

![Omówienie aplikacji interfejsu API](./media/app-service-api-whats-changed/api-apps-overview.png)

Zasada projektowania klucza przy użyciu aktualizacji aplikacji interfejsu API jest aby możliwe było wyświetlić z interfejsu API jest w wybranym języku.  Jeśli z interfejsu API jest już wdrożony w przeglądarce lub aplikacji Mobile, nie musisz ponownie wdróż Twojej aplikacji, aby można było korzystać z nowych funkcji. Jeśli obecnie korzystasz z aplikacji interfejsu API Podgląd, poniżej jest szczegółowe wskazówki dotyczące migracji.

### <a name="authentication"></a>Uwierzytelnianie
Istniejące gotowych aplikacji interfejsu API, aplikacje usług Mobile i aplikacji sieci Web uwierzytelnianie funkcje zostały unified i są dostępne w jednym karta uwierzytelniania Azure aplikacji usługi w portalu zarządzania. Wprowadzenie do usług uwierzytelniania w aplikacji usługi, zobacz [Rozszerzanie aplikacji usługi uwierzytelniania i autoryzacji](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

W przypadku scenariuszy interfejsu API istnieje szereg odpowiednich nowe funkcje:

- **Obsługa przy użyciu Azure usługi Active Directory bezpośrednio**, bez konieczności wymiany token AAD tokenu sesji kod klienta: klient po prostu mogą zawierać tokeny AAD w nagłówku autoryzacji Specyfikacja token okaziciela. Również oznacza to, że nie SDK specyficzne dla aplikacji usługi jest wymagane po stronie klienta i serwera. 
- **Do usługi lub access "Wewnętrznego"**: Jeśli masz proces demona lub innego klienta wymagających dostęp do interfejsów API bez interfejsu można uzyskać token przy użyciu kapitału usługi AAD i przekazać je do aplikacji usługi uwierzytelniania z aplikacją.
- **Autoryzacja opóźniony**: wiele aplikacji ma różne ograniczenia dostępu dla różnych części aplikacji. Być może chcesz niektóre funkcje interfejsu API być publicznie dostępne, a inne wymagają logowania. Oryginalny funkcja uwierzytelniania i autoryzacji była all-or-nothing, z całej witryny wymaganie logowania. Ta opcja nadal istnieje, ale również umożliwia kodzie aplikacji do decydowania programu access po aplikacji usługi został uwierzytelniony użytkownik.
 
Aby uzyskać więcej informacji na temat nowych funkcji uwierzytelniania Zobacz [uwierzytelniania i autoryzacji interfejsu API aplikacji w usłudze Azure aplikacji](app-service-api-authentication.md). Aby uzyskać informacje o migrowaniu istniejące aplikacje interfejsu API z poprzedniego interfejsu API modelu aplikacje na nową, zobacz [Migrowanie istniejącej aplikacji interfejsu API](#migrating-existing-api-apps) w dalszej części tego artykułu.
 
### <a name="cors"></a>CORS
Zamiast rozdzielany przecinkami **MS_CrossDomainOrigins** aplikacji ustawienie, jest teraz karta w portalu zarządzania Azure konfigurowania CORS. Ponadto można skonfigurować, za pomocą Menedżera zasobów narzędzie takich jak Azure programu PowerShell, polecenie lub [Eksploratora zasobów](https://resources.azure.com/). Ustaw właściwość **cors** typ zasobu **Microsoft.Web/sites/config** dla usługi ** &lt;nazwą witryny&gt;/siecią web** zasobów. Na przykład:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>Interfejs API metadanych
Karta definicji interfejsu API jest teraz dostępny w sieci Web, Mobile i interfejsu API aplikacji. W portalu zarządzania można określić względny adres url lub bezwzględny adres url wskazujący na punktu końcowego tej reprezentacja hosts 2.0 Swagger z interfejsu API. Ponadto można skonfigurować, za pomocą narzędzia Menedżer zasobów. Ustaw właściwość **apiDefinition** typ zasobu **Microsoft.Web/sites/config** dla swojego ** &lt;nazwa witryny&gt;/sieci web** zasobów. Na przykład:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

W tej chwili punkt końcowy metadanych musi być publicznie bez uwierzytelniania dla wielu klientów podrzędne (Generowanie klient np Visual Studio interfejsu API usługi REST i przepływu "Dodaj interfejsu API" PowerApps), aby go używać. Oznacza to, jeśli używasz aplikacji usługi uwierzytelniania i udostępnić definicję interfejsu API z samej aplikacji, należy skorzystać z opcji uwierzytelniania wstrzymana opisanych wcześniej było trasy do metadanych Swagger publicznej.

## <a name="management-portal"></a>Portal zarządzania
Wybieranie **Nowy > Web + Mobile > aplikacji interfejsu API** w portalu spowoduje utworzenie interfejsu API aplikacje, które odzwierciedlają nowe funkcje opisane w artykule. **Przejdź > aplikacje interfejsu API** zostaną wyświetlone tylko te aplikacje dla nowego interfejsu API. Po przejściu do aplikacji interfejsu API karta udostępnia tego samego układu i możliwości jak sieci Web i aplikacji Mobile. Tylko różnice są zawartości Szybki Start i porządkowanie ustawień.

Istniejące interfejsu API (aplikacji lub aplikacji interfejsu API Marketplace utworzone na podstawie warunków logicznych aplikacji) z poprzedniej wersji Preview możliwości nadal będą widoczne w Projektancie aplikacji logiki i podczas przeglądania wszystkie zasoby w grupie zasobów.

## <a name="visual-studio"></a>Programu Visual Studio

Większość aplikacji sieci Web narzędzie działa z nowych aplikacji interfejsu API od ich udostępnianie tego samego typu zasobów źródłowych **Microsoft.Web/sites** . Azure Visual Studio narzędzie, należy jednak uaktualnionej wersji 2.8.1 lub nowszym od jej udostępnia wiele możliwości specyficzne dla interfejsów API. Pobierz zestaw SDK ze [strony pobierania Azure](https://azure.microsoft.com/downloads/).

Usługa racjonalizacji typy aplikacji usługi Publikowanie jest również unified w obszarze **Publikuj > Microsoft Azure aplikacji usługi**:

![Publikowanie aplikacji interfejsu API](./media/app-service-api-whats-changed/api-apps-publish.png)

Aby dowiedzieć się więcej na temat SDK 2.8.1, przeczytaj ogłoszenie [w blogu](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Można też ręcznie Importowanie profilu publikowania za pomocą portalu zarządzania, aby włączyć publikowanie. Jednak wymaga SDK 2.8.1 w chmurze Eksploratora, generowanie kodu i aplikacji interfejsu API zaznaczenia i tworzenia lub nowszym.

## <a name="migrating-existing-api-apps"></a>Migrowanie istniejącej aplikacji interfejsu API
Wdrożenie programu niestandardowego interfejsu API starszej wersji Preview w aplikacji interfejsu API prosimy przeprowadzenie migracji do nowego modelu dla aplikacji interfejsu API przez 31 grudnia 2015 r. Ponieważ zarówno stare i nowe modelu są podstawą API sieci Web hostowanej w aplikacji usługi, może nastąpić większość istniejącego kodu.

### <a name="hosting-and-redeployment"></a>Hosting i ponownego rozmieszczania
Instrukcje dotyczące ponowne rozmieszczanie są takie same jak wszystkich istniejących API sieci Web do usługi aplikacji. Kroki:

1. Tworzenie pustego aplikacji interfejsu API. Można to zrobić w portalu z nowy > aplikacji interfejsu API programu Visual Studio z Publikuj lub narzędzia Menedżer zasobów. Jeśli przy użyciu narzędzia Menedżera zasobów lub szablonów, ustaw wartość **typu** **API** o typie zasobu **Microsoft.Web/sites** mają Przewodniki Szybki Start i ustawień w portalu zarządzania kolumnowo kierunku scenariusze interfejsu API.
2. Nawiązywanie połączenia i wdrażanie projektu do pustego aplikacji interfejsu API przy użyciu jednej z mechanizmy wdrażania obsługiwana przez usługę aplikacji. Przeczytaj [dokumentacji wdrażania usługi aplikacji Azure](../app-service-web/web-sites-deploy.md) , aby dowiedzieć się więcej. 
  
### <a name="authentication"></a>Uwierzytelnianie
Usług uwierzytelniania aplikacji usługi pomocy technicznej takie same możliwości, które były dostępne z modelem poprzedniego interfejsu API aplikacje. Jeśli używasz tokeny sesji i wymagają SDK, użyj następujących SDK klienta i serwera:

- Klient: [Azure klientów przenośnych SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Serwer: [Rozszerzenie uwierzytelniania .NET aplikacji dla urządzeń przenośnych Microsoft Azure](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Zamiast tego za pomocą aplikacji usługi alfa SDK, są one teraz przestarzałe:

- Klient: [SDK AppService platformy Microsoft Azure](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Serwer: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

W szczególności z usługi Azure Active Directory, jednak nie specyficznych aplikacji jest wymagane Jeśli używasz AAD token bezpośrednio.

### <a name="internal-access"></a>Dostęp do
Z poprzedniego modelu aplikacje interfejsu API uwzględniane poziom wbudowanego dostępu wewnętrzny. Korzystanie z zestawu SDK to wymagane do żądania podpisania. Sposób opisany w nowym modelu aplikacje interfejsu API głównych usługi AAD może służyć jako alternatywny uwierzytelniania do usługi bez konieczności aplikacji specyficznych SDK. Dowiedz się więcej o [usługi kapitału uwierzytelniania dla aplikacji interfejsu API usługi aplikacji Azure](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Odnajdowanie
Z poprzedniego modelu aplikacje interfejsu API sprzedał interfejsy API za odkrycie inne aplikacje interfejsu API w czasie rzeczywistym w tej samej grupy zasobów za tej samej bramie. To jest szczególnie przydatne w architekturze implementujących wzorców microservice. Gdy to nie jest obsługiwana bezpośrednio, dostępnych jest kilka opcji:

1. Za pomocą interfejsu API Menedżera zasobów Azure dla odnajdowania.
2. Umieszczanie zarządzania interfejsu API Azure przed interfejsów API usługi hostowanej aplikacji usługi. Azure zarządzania interfejsu API służy jako elewacją i umożliwiają stabilny zewnętrznych przeciwległych adresu url, nawet jeśli ulegnie zmianie wewnętrznych topologii.
3. Tworzenie własnych aplikacji interfejsu API odnajdowanie i inne aplikacje interfejsu API zarejestrować przy użyciu aplikacji odnajdowanie podczas uruchamiania.
4. W czasie rozmieszczania wypełniać ustawienia aplikacji wszystkich aplikacji interfejsu API (i klienci) punkty końcowe innych aplikacji interfejsu API. To jest rentowny we wdrożeniach szablonu i ponieważ aplikacje interfejsu API umożliwiają teraz kontrolę nad adresu url.

## <a name="using-api-apps-with-logic-apps"></a>Korzystanie z aplikacji interfejsu API z aplikacjami warunków logicznych

Nowy model interfejsu API w aplikacji działa również z [Aplikacjami logiczny wersji schematu 2015-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej, przeczytaj artykuły w [sekcji dokumentacji aplikacji interfejsu API](https://azure.microsoft.com/documentation/services/app-service/api/). One zostały zaktualizowane, aby odzwierciedlała nowy model dla aplikacji interfejsu API. Ponadto docieranie na forach, aby uzyskać dodatkowe informacje i wskazówki dotyczące migracji:

- [Forum w witrynie MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Przepełnienie stosu](http://stackoverflow.com/questions/tagged/azure-api-apps)
