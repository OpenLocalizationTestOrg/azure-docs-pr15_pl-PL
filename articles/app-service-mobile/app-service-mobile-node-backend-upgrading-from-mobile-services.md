<properties
    pageTitle="Uaktualnienie usług mobilnych usłudze Azure aplikacji — Node.js"
    description="Dowiedz się, jak łatwo uaktualnić aplikacji usług Mobile aplikacji Mobile aplikacji usługi"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Uaktualnianie istniejących usługi mobilnej Azure Node.js do aplikacji usługi

Aplikacja usługi Mobile jest nowy sposób tworzenia aplikacji dla urządzeń przenośnych za pomocą Microsoft Azure. Aby uzyskać więcej informacji, zobacz [Co to są aplikacje Mobile?].

W tym temacie opisano, jak przeprowadzić uaktualnienie istniejącą aplikację Node.js wewnętrznej bazy danych z usługi Azure Mobile do nowej aplikacji Mobile usługi aplikacji. Podczas wykonywania uaktualniania istniejącej aplikacji usług Mobile będą działać.  Jeśli chcesz uaktualnić aplikację Node.js wewnętrznej bazy danych, zapoznaj się z [uaktualniania usługi Mobile .NET](./app-service-mobile-net-upgrading-from-mobile-services.md).

Gdy przenośnych wewnętrznej bazy danych jest uaktualniany do Azure aplikacji usługi, ma dostęp do wszystkich funkcji aplikacji usługi i rozliczenia obsługuje według [ceny aplikacji usługi], nie usługi Mobile ceny.

## <a name="migrate-vs-upgrade"></a>Migrowanie rozbudowy

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Zaleca się tym [Przeprowadzanie migracji](app-service-mobile-migrating-from-mobile-services.md) przed rozpoczęciem uaktualnienia. W ten sposób można umieścić obie wersje aplikacji na tym samym Plan aplikacji usługi i ponoszenia bez dodatkowych opłat.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Ulepszenia w server Mobile aplikacje Node.js SDK

Uaktualnianie do nowego [Zestawu SDK aplikacje Mobile](https://www.npmjs.com/package/azure-mobile-apps) udostępnia wiele udoskonaleń, w tym:

- W zależności od [Express framework](http://expressjs.com/en/index.html), nowe SDK węzeł jest uproszczonej i zaprojektowane w celu zapewnienia nowe wersje węzeł w miarę ich powstawania. Można dostosować zachowanie aplikacji z pośredniczącym Express.

- Ulepszenia dotyczące wydajności istotne w porównaniu z SDK usług Mobile.

- Teraz możesz hostować witrynę sieci Web razem z sieci wewnętrznej bazy danych urządzeń przenośnych; Podobnie jest łatwe dodawanie Azure Mobile SDK do istniejącej aplikacji express.v4.

- Utworzono dla rozwoju i platform i lokalnego, SDK aplikacje Mobile można opracowanych i uruchomić lokalnie na platformach Windows, Linux i OSX. Teraz jest łatwe w użyciu typowych węzeł metody uruchamiania testów [Mocha](https://mochajs.org/) przed wdrożeniem.

## <a name="overview"></a>Omówienie uaktualniania

Pomocnych w wewnętrznej bazie danych Node.js uaktualniania Azure aplikacji usługi udostępniła pakiet zgodności.  Po uaktualnieniu masz witryny niew, które mogą być wdrażane do nowej witryny aplikacji usługi.

Klient usług Mobile SDK są **niezgodny z nowym serwerem aplikacji Mobile SDK** . Aby zapewnić ciągłość usługi dla aplikacji, nie należy opublikować zmiany w witrynie obsługujący opublikowanych klientów. Zamiast tego należy utworzyć nowej aplikacji dla urządzeń przenośnych pełniącej funkcję duplikat. Ta aplikacja można umieścić na tego samego planu aplikacji usługi, aby uniknąć ponoszenia dodatkowych finansowa.

Następnie, dostępne będą dwie wersje aplikacji: aplikacje pozostaje taki sam i służy opublikowanych w galerii znaków symboli i inne, co można uaktualnić i docelowej przy użyciu nowej wersji klienta. Można przenieść i przetestować kod w swojej tempie, ale należy się upewnić, że wszelkie poprawki wprowadzone zostać zastosowane do obu. Gdy uznasz, że żądaną liczbę klienta, które aplikacje w warunkach naturalnych zostały zaktualizowane do najnowszej wersji, możesz usunąć oryginalny aplikacji migracją Jeśli jest to wymagane. Wszelkie dodatkowe koszty pieniężnych go nie spowodować, jeśli obsługiwany w tego samego planu usługi aplikacji jako aplikacji Mobile.

Pełna konspektu w procesie uaktualniania jest następująca:

1. Pobierz usługi istniejących Mobile Azure (migracją).
2. Konwertowanie projektu aplikacji Mobile Azure za pomocą pakietu zgodności.
3. Popraw różnice (na przykład ustawienia uwierzytelniania).
4. Wdrażanie przekonwertowane projektu aplikacji Mobile Azure nowej usługi aplikacji.
4. Zwolnij nową wersję aplikacji klienta, którego używać nowej aplikacji Mobile.
5. (Opcjonalnie) Usuń oryginalny aplikacji migracją usług mobilnych.

Usunięcie mogą wystąpić w przypadku na oryginalny migracją usług mobilnych nie ma żadnego ruchu.

## <a name="install-npm-package"></a>Instalowanie wymagania wstępne

[Węzeł] należy zainstalować na komputerze lokalnym.  Należy również zainstalować pakiet zgodności.  Po zainstalowaniu węzeł można uruchomić następujące polecenie z nowego cmd lub w wierszu polecenia programu PowerShell:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Uzyskać skrypty Azure usług mobilnych

- Zaloguj się do [portalu Azure].
- Za pomocą **Wszystkich zasobów** lub **Aplikacji usługi**, Znajdź witryny usług Mobile.
- W witrynie, kliknij menu **Narzędzia** -> **Kudu** -> **Przejdź** , aby otworzyć witrynę Kudu.
- Kliknij na **Konsoli debugowania** -> **programu PowerShell** , aby otworzyć konsolę debugowania.
- Przejdź do `site/wwwroot/App_Data/config` , klikając kolejno na każdym katalogu
- Kliknij ikonę pobierania obok pozycji `scripts` katalogu.

Spowoduje to pobranie skryptów w formacie ZIP.  Utwórz nowy katalog na komputerze lokalnym i rozpakowywanie `scripts.ZIP` pliku w katalogu.  Spowoduje to utworzenie `scripts` katalogu.

## <a name="scaffold-app"></a>Scaffold nowej aplikacji Mobile Azure wewnętrznej bazy danych

Z poziomu katalogu zawierającego katalog skryptów, uruchom następujące polecenie:

```scaffold-mobile-app scripts out```

Spowoduje to utworzenie scaffolded wewnętrznej bazie danych aplikacji Mobile Azure w `out` katalogu.  Mimo że nie jest to wymagane, jest dobrym pomysłem jest sprawdzanie `out` katalogu do repozytorium kodu źródłowego wybranych przez użytkownika.

## <a name="deploy-ama-app"></a>Wdrażanie usługi aplikacji Mobile Azure wewnętrznej bazy danych

Podczas wdrażania należy wykonać następujące czynności:

1. Tworzenie nowej aplikacji Mobile w [Azure Portal].
2. Uruchamianie `createViews.sql` skrypt połączenia bazy danych.
3. Łączenie bazy danych, która jest połączony z usługą Mobile do nowej usługi aplikacji.
4. Inne zasoby (na przykład powiadomienie koncentratory) łącze do nowej usługi aplikacji.
5. Wdrażanie wygenerowanego kodu do nowej witryny.

### <a name="create-a-new-mobile-app"></a>Tworzenie nowej aplikacji Mobile

1. Zaloguj się w [portalu Azure].

2. Kliknij pozycję **+ Nowe** > **Web + Mobile** > **Aplikacji Mobile**, następnie podaj nazwę dla twojej aplikacji Mobile wewnętrznej bazy danych.

3. **Grupa zasobów**wybierz istniejącej grupy zasobów lub utworzyć nową (pod tą samą nazwą jako aplikacji). 
 
    Można wybrać inny plan usługi aplikacji lub Utwórz nowy. Aby uzyskać więcej informacji o usługach aplikacji planów i jak utworzyć nowy plan w innej ceny warstwa i w odpowiedniej lokalizacji, zobacz [Omówienie szczegółowo planów Azure aplikacji usługi](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Plan usług aplikacji**domyślny plan (w [standardowej warstwa](https://azure.microsoft.com/pricing/details/app-service/)) jest zaznaczone. Można także wybrać innego planu, lub [Utwórz nowy](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Planowanie aplikacji usługi ustawień określają [lokalizację, funkcje, kosztów i obliczanie zasoby](https://azure.microsoft.com/pricing/details/app-service/) skojarzone z aplikacji. 

    Po określeniu w planie, kliknij przycisk **Utwórz**. Spowoduje to utworzenie aplikacji Mobile wewnętrznej bazy danych. 


### <a name="run-createviewssql"></a>Uruchamianie CreateViews.SQL

Aplikacja scaffolded znajduje się plik o nazwie `createViews.sql`.  Ten skrypt muszą być wykonane w docelowej bazie danych.  Parametry połączenia dla docelowej bazy danych można uzyskać z migracją usługi urządzeń przenośnych z karta **Ustawienia** w obszarze **Parametry połączenia**.  Ma nazwę `MS_TableConnectionString`.

Możesz uruchomić ten skrypt w programie SQL Server Management Studio lub Visual Studio.

### <a name="link-the-database-to-your-app-service"></a>Łączenie bazy danych aplikacji usługi

Łącze istniejącej bazy danych do usługi aplikacji:

- Otwórz [Azure Portal]usługi aplikacji.
- Zaznacz **wszystkie ustawienia** -> **połączenia danych**.
- Polecenie **+ Dodaj**.
- Z listy rozwijanej wybierz **Bazę danych SQL**
- W obszarze **Bazy danych SQL**wybierz istniejącej bazy danych, a następnie wybierz polecenie **Wybierz**.
- W obszarze **Parametry połączenia**wprowadź nazwę użytkownika i hasło dla bazy danych, a następnie kliknij przycisk **OK**.
- W karta **Dodaj połączenia danych** kliknij **przycisk OK**.

Nazwy użytkownika i hasła znajdują się, wyświetlając parametry połączenia dla docelowej bazy danych w usłudze migracją Mobile.


### <a name="set-up-authentication"></a>Konfigurowanie uwierzytelniania

Azure aplikacji Mobile umożliwia konfigurowanie Azure Active uwierzytelniania katalogu, Facebook, Google, Microsoft i Twitter w usłudze.  Niestandardowe uwierzytelnianie muszą być rozwinięte oddzielnie.  Można znaleźć w dokumentacji [Pojęcia uwierzytelniania] i dokumentacji [Uwierzytelniania Szybki Start] , aby uzyskać więcej informacji.  

## <a name="updating-clients"></a>Aktualizowanie klientów przenośnych

Po umieszczeniu operacyjne wewnętrznej bazy danych aplikacji Mobile, można pracować nad nową wersję aplikacji klienta, który używa go. Aplikacje Mobile zawiera również nowa wersja klienta SDK i podobne do uaktualnienia serwera powyżej, konieczne będzie usunięcie wszystkich odwołań do SDK usług Mobile przed zainstalowaniem wersji aplikacji Mobile.

Jedną z główne zmiany między wersjami jest to, że konstruktory już nie wymagają klawisz aplikacji. Możesz teraz po prostu przekazać adres URL aplikacji Mobile. Na przykład w klientach .NET `MobileServiceClient` Konstruktor jest teraz:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Można przeczytać o instalowaniu nowego SDK i używaniu nową strukturę za pośrednictwem z poniższych łączy:

- [Android w wersji 2,2 lub nowszej](app-service-mobile-android-how-to-use-client-library.md)
- [iOS wersji 3.0.0 lub nowszym](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows-Xamarin) wersji 2.0.0 lub nowszym](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova w wersji 2.0 lub nowszy](app-service-mobile-cordova-how-to-use-client-library.md)

Jeśli aplikacja jest stosowanie powiadomienia wypychane, zanotuj instrukcje określonej rejestracji dla każdej z tych platform, jak nie są dostępne niektóre zmiany także.

Jeśli masz nowa wersja klienta gotowa, przetestuj przed projektu uaktualnionego serwera. Po sprawdzania, czy działa, możesz zwolnić nowej wersji aplikacji do klientów. Po pewnym czasie po klientów mieć możliwość odbierania tych aktualizacji, można usunąć wersji usługi Mobile aplikacji. W tym momencie uaktualnieniu do aplikacji Mobile usługi dla aplikacji przy użyciu najnowszych serwera aplikacji Mobile SDK.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Co to są aplikacje Mobile?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Cennik aplikacji usługi]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Pojęcia uwierzytelniania]: ../app-service/app-service-authentication-overview.md
[Szybki Start uwierzytelniania]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
