<properties
    pageTitle="Jak pracować z serwera wewnętrznej bazy danych Node.js SDK dla aplikacji Mobile | Azure aplikacji usługi"
    description="Dowiedz się, jak pracować z serwera wewnętrznej bazy danych Node.js SDK dla Mobile Azure aplikacji usługi."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Jak używać SDK Node.js aplikacje Mobile Azure

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Ten artykuł zawiera szczegółowe informacje i przykłady przedstawiający, jak pracować z Node.js wewnętrznej bazy danych w aplikacji Mobile Azure aplikacji usługi.

## <a name="Introduction"></a>Wprowadzenie

Azure usługi aplikacji Mobile umożliwia dodawanie dostępu do danych zoptymalizowanej pod mobile interfejs API sieci Web do aplikacji sieci web.  Azure aplikacji usługi Mobile aplikacje SDK jest dostępne dla aplikacji sieci web programu ASP.NET i Node.js.  Zestaw SDK zawiera następujące operacje:

- Operacje tabeli (Odczyt, Usuń Insert, Update) uzyskać dostęp do danych
- Operacje niestandardowe interfejsu API

Obie operacje zapewnienie uwierzytelniania dla wszystkich dostawców tożsamości zezwala na to Azure aplikacji usługi, w tym dostawcy tożsamości społecznościowych, takich jak Facebook, Twitter, Google i firmy Microsoft, jak również usługi Azure Active Directory dla tożsamości przedsiębiorstwa.

Przykłady można znaleźć w każdym przypadku użycia w [katalogu próbki na GitHub].

## <a name="supported-platforms"></a>Platformy obsługiwane

Azure Mobile aplikacje węzeł SDK obsługuje bieżącej wersji KÓW węzła lub w nowszej wersji.  Od pisania, najnowsza wersja KÓW jest v4.5.0 węzeł.  Inne wersje węzeł może działać, ale nie są obsługiwane.

Azure Mobile aplikacje węzeł SDK obsługuje dwa sterowników bazy danych — sterownik mssql węzeł obsługuje platformy SQL Azure i lokalnych wystąpienia programu SQL Server.  Sterownik sqlite3 obsługuje SQLite baz danych na tylko jedno wystąpienie.

### <a name="howto-cmdline-basicapp"></a>Jak: Tworzenie podstawowego Node.js wewnętrznej bazie danych przy użyciu wiersza polecenia

Jako aplikacja ExpressJS zaczyna się każdy Azure aplikacji usługi Mobile aplikacji Node.js wewnętrznej bazy danych.  ExpressJS jest dostępna dla Node.js infrastrukturze usługi najpopularniejszych sieci web.  Można utworzyć basic [Express] aplikacji w następujący sposób:

1. W oknie programu PowerShell lub polecenie Utwórz katalog projektu.

        mkdir basicapp

2. Uruchom npm inicjowania, aby zainicjować struktury pakiet.

        cd basicapp
        npm init

    Polecenie inicjowania npm zapytanie zestaw pytań, aby zainicjować projektu.  Przykładowe dane wyjściowe, zobacz:

    ![Wynik inicjowania npm][0]

3. Zainstaluj bibliotek ekspresowa i aplikacje azure-mobile z repozytorium npm.

        npm install --save express azure-mobile-apps

4. Utwórz plik app.js do wykonania podstawowy serwer urządzeń przenośnych.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Ta aplikacja tworzy WebAPI zoptymalizowane mobile o jeden punkt końcowy (`/tables/TodoItem`) zawierający nieuwierzytelnionych dostęp do podstawowej magazynu danych SQL za pomocą schematu dynamiczne.  Jest ona odpowiednia dla po klienta biblioteki Szybkie uruchamianie:

- [Szybki Start android klienta]
- [Szybki Start klienta Cordova Apache]
- [iOS klienta Szybki Start]
- [Szybki Start klienta ze Sklepu Windows]
- [Szybki Start Xamarin.iOS klienta]
- [Szybki Start Xamarin.Android klienta]
- [Szybki Start Xamarin.Forms klienta]

Kod można znaleźć w tej aplikacji podstawowe w [próbce basicapp na GitHub].

### <a name="howto-vs2015-basicapp"></a>Jak: tworzenie węzeł wewnętrznej bazy danych przy użyciu programu Visual Studio 2015 r.

Visual Studio 2015 wymaga rozszerzenia do tworzenia aplikacji Node.js w IDE.  Aby rozpocząć, zainstaluj [Node.js 1.1 narzędzia programu Visual Studio].  Po zainstalowaniu narzędzia Node.js programu Visual Studio, utworzyć aplikację Express 4.x:

1. Otwórz okno dialogowe **Nowy projekt** (z **pliku** > **Nowy** > **projektu...**).

2. Rozwiń listę **Szablony** > **JavaScript** > **Node.js**.

3. Wybierz **aplikację podstawowe Azure Node.js Express 4**.

4. Wpisz nazwę projektu.  Kliknij *przycisk OK*.

    ![Nowy projekt Visual Studio 2015 r.][1]

5. Kliknij prawym przyciskiem myszy węzeł **npm** i wybierz pozycję **Zainstaluj nowe pakiety npm...**.

6. Może być konieczne odświeżenie wykazu npm na tworzenie pierwszej aplikacji Node.js.  Jeśli to konieczne, kliknij przycisk **Odśwież** .

7. Wprowadź _aplikacji w przypadku mobile azure_ w polu wyszukiwania.  Kliknij pakiet **azure mobile aplikacje 2.0.0** , a następnie kliknij pozycję **Zainstaluj pakiet**.

    ![Instalowanie nowych pakietów npm][2]

8. Kliknij przycisk **Zamknij**.

9. Otwórz plik _app.js_ , aby dodać obsługę SDK aplikacje Mobile Azure.  W wierszu 6 at dolnej części biblioteki wymagają instrukcji, Dodaj następujący kod:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    W przybliżeniu wierszu 27 po innych instrukcji app.use Dodaj następujący kod:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Zapisz plik.

10. Uruchom aplikację lokalnie (API jest obsługiwana w http://localhost:3000) albo publikowanie Azure.

### <a name="create-node-backend-portal"></a>Jak: tworzenie Node.js wewnętrznej bazie danych za pomocą portalu Azure

Możesz utworzyć prawo wewnętrznej bazy danych aplikacji Mobile w [Azure portal]. Można wykonać następujące czynności lub Utwórz klienta i serwera ze sobą, wykonując samouczek [Tworzenie aplikacji dla urządzeń przenośnych](app-service-mobile-ios-get-started.md) . Samouczek zawiera uproszczoną wersję te instrukcje i najlepiej odpowiadający dowodu koncepcji projektów.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Po powrocie do karta _wprowadzenie_ w obszarze **Utwórz tabelę interfejsu API**wybierz **Node.js** jako **język wewnętrznej bazy danych**. Zaznacz pole obok ",**I Potwierdź, że spowoduje to zastąpienie wszystkich zawartość witryny.**", a następnie kliknij **TodoItem Tworzenie tabeli**.

### <a name="download-quickstart"></a>Jak: pobieranie przy użyciu cyfra projekt kodu Node.js wewnętrznej bazy danych Szybki Start

Po utworzeniu aplikacji Mobile Node.js wewnętrznej bazy danych za pomocą portalu karta **Szybki start** Node.js projektu jest tworzony i używany do witryny. Można dodać tabele i interfejsów API i edytować pliki kodu dla Node.js wewnętrznej bazy danych w portalu. Za pomocą różnych elementach do pobrania projektu wewnętrznej bazy danych, dlatego możesz dodać lub zmodyfikować tabel oraz interfejsy API, a następnie ponownie opublikować projekt. Aby uzyskać więcej informacji zobacz [Podręcznik wdrażania usługi Azure w aplikacji]. w poniższej procedurze użyto repozytorium cyfra, aby pobrać kodu projektu Szybki Start.

1. Zainstaluj cyfra, jeśli jeszcze tego nie zrobiono. Kroki wymagane do zainstalowania cyfra różnić systemy operacyjne. Zobacz [Instalowanie cyfra](http://git-scm.com/book/en/Getting-Started-Installing-Git) dla konkretnych systemów operacyjnych dystrybucji i instalacji wskazówek.

2. Postępuj zgodnie z instrukcjami [Włącz repozytorium aplikacji usługi aplikacji](../app-service-web/app-service-deploy-local-git.md#Step3) umożliwiające repozytorium cyfra witryny wewnętrznej bazy danych, tworzenie notatki wdrożenia nazwy użytkownika i hasła.

3. Zanotuj ustawienie **adresu URL klonowanie cyfra** z karta dla twojej aplikacji Mobile wewnętrznej bazy danych.

4. Wykonywanie `git clone` polecenia za pomocą cyfra klonowanie adresu URL, wprowadzanie hasła, gdy jest to wymagane, tak jak w poniższym przykładzie:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Przejdź do katalogu lokalnego, czyli w poprzednim przykładzie /todolist i zwróć uwagę, że zostały pobrane pliki projektu. Znajdź `todoitem.json` plików w `/tables` katalogu.  Ten plik definiuje uprawnienia w tabeli.  Również znaleźć `todoitem.js` plik w tym samym katalogu, który definiuje tej operacji OBSŁUGIWAŁ skrypty dla tabeli.

6. Po wprowadzeniu zmian do plików projektu, wykonaj następujące polecenia, aby dodać, Zatwierdź, a następnie przekazać zmiany do witryny:

        $ git commit -m "updated the table script"
        $ git push origin master

    Po dodaniu nowych plików do projektu, najpierw trzeba wykonać `git add .` polecenia.

Witryny opublikowaniu każdorazowo nowy zestaw zatwierdzenia zostanie przypisany do witryny.

### <a name="howto-publish-to-azure"></a>Jak: publikowanie do wewnętrznej bazy danych Node.js Azure

Microsoft Azure udostępnia wiele mechanizmy do publikowania usługi Azure aplikacji usługi Mobile aplikacje Node.js wewnętrznej bazy danych z usługą Azure.  Dotyczy to korzystanie z narzędzia do wdrażania zintegrowany z programu Visual Studio, narzędzi wiersza polecenia i opcje wdrażania ciągły na podstawie źródło formantu.  Aby uzyskać więcej informacji na ten temat zobacz [Podręcznik wdrażania usługi Azure w aplikacji].

Azure usługa aplikacji ma konkretne porady dla aplikacji Node.js, które należy przejrzeć przed wdrożeniem:

- Jak [określić wersję węzeł]
- Jak [używać moduły węzeł]

### <a name="howto-enable-homepage"></a>Jak: Włączanie strony głównej aplikacji

Wiele aplikacji są kombinacją sieci web i aplikacji dla urządzeń przenośnych i struktury ExpressJS umożliwia łączenie dwóch aspekty.  Czasem jednak możesz przeprowadzić tylko interfejsu dla urządzeń przenośnych.  Zaleca się zapewnić, że jest strona początkowa, aby zapewnić aplikacji usługi i rozpocząć pracę.  Można podać strony głównej lub włączyć tymczasowe strony głównej.  Aby włączyć tymczasowe strona główna, wystąpienia aplikacji Mobile Azure należy wykonać następujące kroki:

    var mobile = azureMobileApps({ homePage: true });

Jeśli chcesz tylko ta opcja dostępna podczas tworzenia lokalnie, możesz dodać to ustawienie, aby usługi `azureMobile.js` pliku.

## <a name="TableOperations"></a>Operacje tabeli 

Aplikacje w przypadku mobile azure Node.js Server SDK udostępnia mechanizmy udostępniania tabel danych przechowywanych w bazie danych SQL Azure jako WebAPI.  Pięć operacji są dostarczane.

| Operacja | Opis |
| --------- | ----------- |
| Uzyskiwanie /tables/_tablename_ | Pobierz wszystkie rekordy w tabeli |
| Uzyskiwanie /tables/_tablename_/:id | Uzyskiwanie określonego rekordu w tabeli |
| WPIS /tables/_tablename_ | Tworzenie rekordu w tabeli |
| POPRAWKA /tables/_tablename_/:id | Zaktualizuj rekord w tabeli |
| Usuwanie /tables/_tablename_/:id | Usuwanie rekordu w tabeli |

Ten WebAPI obsługuje [OData] i rozszerza schemat tabeli do obsługi [synchronizacji danych w trybie offline].

### <a name="howto-dynamicschema"></a>Jak: Definiowanie tabel za pomocą dynamicznego schematu

Zanim będzie można używać tabeli, należy zdefiniować.  Tabele można definiować ze schematem statyczny (gdzie projektanta definiuje kolumny w schemacie) lub dynamicznie (gdzie SDK kontroluje schemat oparty na przychodzących wezwań). Ponadto projektanta można kontrolować określonych aspektów WebAPI przez dodanie kodu Javascript do definicji.

Zgodnie z zaleceniami dotyczącymi możesz należy zdefiniować każdej tabeli w pliku Javascript w katalogu tabele, a następnie zaimportuj tabele za pomocą metody tables.import().  Rozszerzanie aplikacji basic, czy dostosowanie pliku app.js:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Definiowanie tabeli. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabele używać schematu dynamiczne domyślnie.  Aby wyłączyć dynamiczne schematu globalnie, ustaw ustawienie aplikacji **MS_DynamicSchema** ma wartość FAŁSZ w Azure portal.

Rozbudowany przykład można znaleźć w [próbce zadania na GitHub].

### <a name="howto-staticschema"></a>Jak: Definiowanie tabel za pomocą schematów statycznych

Można jawnie zdefiniować kolumny, które chcesz udostępnić za pośrednictwem WebAPI.  Aplikacje azure-mobile SDK Node.js automatycznie dodaje wszelkie dodatkowe kolumny wymagane dla synchronizacja danych w trybie offline, aby liście podasz.  Na przykład aplikacje klienckie Szybki Start wymagać tabeli z dwiema kolumnami: tekst (ciąg) i wypełnij (wartość logiczna).  
Tabelę można zdefiniować w tabeli definicji pliku JavaScript (znajdujące się w katalogu tabel) w następujący sposób:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Jeśli zdefiniujesz statycznie tabel, musisz również wywołać metodę tables.initialize(), aby utworzyć schemat bazy danych po ponownym uruchomieniu komputera.  Metoda tables.initialize() zwraca [zobowiązanie] tak, aby usługa sieci web nie obsługiwać żądania przed inicjowania bazy danych.

### <a name="howto-sqlexpress-setup"></a>Jak: używanie programu SQL Express rozwoju przechowywania danych na komputerze lokalnym

Azure Mobile aplikacje AzureMobile aplikacje węzeł zestawu SDK zawiera trzy opcje do obsługi danych poza pole: zestaw SDK zawiera trzy opcje obsługi danych poza pole:

- Umożliwia uzyskanie nietrwałe przykład sklep sterownik **pamięci**
- Za pomocą sterownika **mssql** stanowią magazyn danych programu SQL Express rozwoju
- Za pomocą sterownika **mssql** zapewnienie magazynu danych bazy danych SQL Azure produkcji

SDK Node.js aplikacje Mobile Azure używa [mssql Node.js pakiet] w celu uznania i użyć połączenia SQL Express i baza danych SQL.  Ten pakiet wymaga Włączanie połączenia TCP na wystąpienia programu SQL Express.

> [AZURE.TIP]Sterownik pamięci nie umożliwia kompletny zestaw urządzenia do testowania.  Jeśli chcesz przetestować wewnętrznej lokalnie z bazy danych, zalecamy użycie magazynu danych programu SQL Express i sterownik mssql.

1. Pobierz i zainstaluj [Program Microsoft SQL Server 2014 Express].  Upewnij się, że zainstalowanie programu SQL Server 2014 Express przy użyciu narzędzia do edycji.  Jeśli nie jest jawnie wymagane obsługę 64-bitową, 32-bitową wersję powoduje zużycie mniej pamięci podczas uruchamiania.

2. Uruchom Menedżer konfiguracji programu SQL Server 2014 r.

  1. Rozwiń węzeł **Konfiguracja sieciowa programu SQL Server** w menu po lewej stronie drzewa.
  2. Kliknij pozycję **Protokoły dla SQLEXPRESS**.
  3. Kliknij prawym przyciskiem myszy **TCP/IP** , a następnie zaznacz pole wyboru **Włącz**.  Kliknij **przycisk OK** w oknie podręcznym.
  4. Kliknij prawym przyciskiem myszy **TCP/IP** , a następnie wybierz pozycję **Właściwości**.
  5. Kliknij kartę **Adresy IP** .
  6. Znajdź węzeł **IPWszystkie** .  W polu **TCP Port** wprowadź **1433**.

         ![Configure SQL Express for TCP/IP][3]

  7. Kliknij **przycisk OK**.  Kliknij **przycisk OK** w oknie podręcznym.
  8. W menu drzewa po lewej stronie kliknij polecenie **Usług SQL Server** .
  9. Kliknij prawym przyciskiem myszy **Program SQL Server (SQLEXPRESS)** i wybierz polecenie **Uruchom ponownie**
  10. Zamknij Menedżer konfiguracji programu SQL Server 2014 r.

3. Uruchom program SQL Server 2014 Management Studio i nawiązać połączenie lokalne wystąpienie programu SQL Express

  1. Kliknij prawym przyciskiem myszy wystąpienia w Eksploratorze obiekt i wybierz polecenie **Właściwości**
  2. Zaznacz stronę, **Zabezpieczenia** .
  3. Upewnij się, że jest zaznaczona pozycja **programu SQL Server i tryb uwierzytelniania systemu Windows**
  4. Kliknij **przycisk OK**

        ![Konfigurowanie uwierzytelniania Express SQL][4]

  5. Rozwiń węzeł **Zabezpieczenia** > **logowania** w Eksploratorze obiektów
  6. Kliknij prawym przyciskiem myszy **logowania** i wybierz pozycję **New Login...**
  7. Wprowadź nazwę logowania.  Wybierz opcję **Uwierzytelnianie programu SQL Server**.  Wprowadź hasło, a następnie wprowadź to samo hasło w polu **Potwierdź hasło**.  Hasło musi spełniać wymagania dotyczące złożoności systemu Windows.
  8. Kliknij **przycisk OK**

        ![Dodawanie nowego użytkownika do programu SQL Express][5]

  9. Kliknij prawym przyciskiem myszy nazwę nowego użytkownika, a następnie wybierz pozycję **Właściwości**
  10. Zaznacz stronę, **Role serwerów**
  11. Zaznacz pole obok rola serwera **Twórcy baz danych**
  12. Kliknij **przycisk OK**
  13. Zamknij SQL Server 2015 Management Studio

Upewnij się, że rekord nazwy użytkownika i hasła, wybrane.  Może być konieczne przypisywanie ról dodatkowy serwer i uprawnień w zależności od potrzeb konkretnej bazy danych.

Aplikacja Node.js odczytuje zmienną środowiska **SQLCONNSTR_MS_TableConnectionString** pod kątem parametry połączenia dla tej bazy danych.  Możesz ustawić tej zmiennej w środowisku.  Za pomocą programu PowerShell można na przykład ustawić tej zmiennej środowiska:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Dostęp do bazy danych za pośrednictwem połączenia TCP/IP i podaj nazwę użytkownika i hasło dla połączenia.

### <a name="howto-config-localdev"></a>Jak: Konfigurowanie projektu rozwoju lokalne

Azure aplikacji Mobile odczytuje plik JavaScript o nazwie _azureMobile.js_ z lokalnego systemu plików.  Nie należy używać tego pliku do konfigurowania SDK aplikacje Mobile Azure produkcji — zamiast tego użyj ustawień aplikacji w [Azure portal] .  Plik _azureMobile.js_ należy wyeksportować obiekt konfiguracji.  Są to najczęściej używane ustawienia:

- Ustawienia bazy danych
- Ustawienia rejestrowania diagnostycznego
- Ustawienia CORS alternatywny

Przykładowy plik _azureMobile.js_ implementacji poprzednich ustawień bazy danych są następujące:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Zaleca się dodawanie _azureMobile.js_ do pliku _.gitignore_ (lub innych Kontrola kodu źródłowego ignorowanie pliku) aby zapobiec hasła są przechowywane w chmurze.  Zawsze należy skonfigurować ustawienia produkcji w obszarze Ustawienia aplikacji w [Azure portal].

### <a name="howto-appsettings"></a>Jak: Konfigurowanie ustawień aplikacji dla urządzeń przenośnych aplikacji

Większość ustawień w pliku _azureMobile.js_ mieć odpowiednik ustawienia aplikacji w [Azure portal].  Poniższa lista umożliwia skonfigurowanie aplikacji w obszarze Ustawienia aplikacji:

| Ustawienia aplikacji                 | Ustawienie _azureMobile.js_  | Opis                               | Prawidłowe wartości                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | Nazwa                      | Nazwa aplikacji                       | ciąg                                      |
| **MS_MobileLoggingLevel**   | Logging.level             | Poziom dziennika minimalne wiadomości do logowania      | błąd, ostrzeżenie o pełne debugowania, niemądre |
| **MS_DebugMode**            | debugowanie                     | Włączanie lub wyłączanie trybu debugowania              | PRAWDA, FAŁSZ                                 |
| **MS_TableSchema**          | Data.Schema               | Domyślna nazwa schematu tabele SQL        | ciąg (domyślny: dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Włączanie lub wyłączanie trybu debugowania              | PRAWDA, FAŁSZ                                 |
| **MS_DisableVersionHeader** | Wersja (ustawiona do niezdefiniowaną)| Wyłącza nagłówku X-ZUMO-— wersja serwera | PRAWDA, FAŁSZ                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Wyłącza sprawdzanie wersji interfejs API klienta     | PRAWDA, FAŁSZ                                 |

Aby ustawić ustawienia aplikacji:

1. Zaloguj się do [portalu Azure].
2. Zaznacz **wszystkie zasoby** lub **Usług aplikacji** , a następnie kliknij nazwę aplikacji Mobile.
3. Domyślnie zostanie wyświetlona karta Ustawienia. W przeciwnym razie kliknij pozycję **Ustawienia**.
4. W menu Ogólne kliknij pozycję **Ustawienia aplikacji** .
5. Przewiń do sekcji Ustawienia aplikacji.
6. Jeśli aplikacji ustawienie już istnieje, kliknij pozycję wartość ustawienia aplikacji, aby edytować wartość.
7. Jeśli ustawienia aplikacji nie istnieje, wprowadź ustawienia aplikacji w polu klucz i wartość w polu wartość.
8. Po zakończeniu, kliknij przycisk **Zapisz**.

Zmienianie ustawień aplikacji większość wymaga ponownego uruchomienia usługi.

### <a name="howto-use-sqlazure"></a>Jak: Użyj bazy danych SQL do przechowywania danych produkcji

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Używanie bazy danych SQL Azure jako magazynu danych jest identyczny przez wszystkie typy aplikacji Azure aplikacji usługi. Jeśli jeszcze tego nie zrobiono, wykonaj poniższe czynności, aby utworzyć aplikacji Mobile wewnętrznej bazie danych.

1. Zaloguj się do [portalu Azure].

2. W górnym lewym rogu okna, kliknij przycisk **+ Nowy** > **Web + Mobile** > **Aplikacji Mobile**podać nazwę swojej wewnętrznej bazy danych aplikacji Mobile.

3. W polu **Grupa zasobów** wprowadź taką samą nazwę jak aplikacji.

4. Plan usług aplikacji domyślny jest zaznaczone.  Jeśli chcesz zmienić plan usługi aplikacji usługi, możesz to zrobić, klikając pozycję Plan usług aplikacji > **+ Utwórz nowe**.  Wprowadź nazwę nowego planu usługi aplikacji i wybierz odpowiednią lokalizację.  Kliknij poziom ceny i wybierz odpowiedni poziom cennik usługi. Wybierz pozycję **Wyświetl wszystkie** , aby wyświetlić więcej opcji cennik, takich jak **bezpłatnego** i **udostępnione**.  Po wybraniu cennik warstwy, kliknij przycisk **Wybierz** .  W programie karta **plan usług aplikacji** kliknij **przycisk OK**.

5. Kliknij przycisk **Utwórz**. Inicjowanie obsługi administracyjnej aplikacji Mobile wewnętrznej bazie danych może potrwać kilka minut.  Po wewnętrznej bazy danych aplikacji Mobile jest obsługi administracyjnej, portalu zostanie wyświetlona karta **Ustawienia** dla aplikacji Mobile wewnętrznej bazy danych.

Po utworzeniu aplikacji Mobile wewnętrznej bazy danych można nawiązać z istniejącą bazą danych SQL do wewnętrznej bazy danych aplikacji Mobile albo tworzenie nowej bazy danych SQL.  W tej sekcji tworzymy z bazą danych SQL.

> [AZURE.NOTE]Jeśli masz już bazy danych, w tym samym miejscu jako wewnętrznej bazy danych aplikacji dla urządzeń przenośnych, możesz zamiast tego wybrać opcję **Użyj istniejącej bazy danych** , a następnie wybierz tej bazy danych. Użyj bazy danych w innej lokalizacji nie jest zalecane ze względu na wyższy opóźnienia.

6. W nowej aplikacji Mobile wewnętrznej bazy danych, kliknij przycisk **Ustawienia** > **Aplikacji Mobile** > **danych** > **+ Dodaj**.

7. W karta **Dodaj połączenie danych** , kliknij przycisk **Baza danych SQL — Konfigurowanie ustawień wymagane** > **Utwórz nową bazę danych**.  Wprowadź nazwę nowej bazy danych w polu **Nazwa** .

8. Kliknij pozycję **serwer**.  W karta **nowego serwera** wprowadź nazwę serwera unikatowe w polu **Nazwa serwera** i podanie odpowiedniego **serwera logowania administratora** i **hasło**.  Upewnij się, że jest zaznaczone pole wyboru **Zezwalaj azure usług dostępu do serwera** .  Kliknij **przycisk OK**.

    ![Tworzenie bazy danych programu Azure SQL][6]

9. Na karta **nowej bazy danych** kliknij **przycisk OK**.

10. Powrót na karta **Dodaj połączenia danych** wybierz pozycję **Parametry połączenia**, wprowadź identyfikator logowania i hasło, podana podczas tworzenia bazy danych.  Jeśli korzystasz z istniejącej bazy danych, należy podać poświadczenia logowania dla tej bazy danych.  Po wpisaniu, kliknij **przycisk OK**.

11. Ponowne zalogowanie karta **Dodaj połączenie danych** , kliknij **przycisk OK** , aby utworzyć bazę danych.

<!--- END OF ALTERNATE INCLUDE -->

Tworzenie bazy danych może potrwać kilka minut.  Monitorowanie postępu wdrażania za pomocą obszaru **powiadomień** .  Nie o postępie do momentu został pomyślnie wdrożony bazy danych.  Po pomyślnym rozmieszczeniu dla wystąpienie bazy danych SQL w swojej przenośnych wewnętrznej bazy danych ustawień aplikacji jest tworzona parametry połączenia.  Zobaczysz to ustawienie aplikacji, w obszarze **Ustawienia** > **Ustawienia aplikacji** > **Parametry połączenia**.

### <a name="howto-tables-auth"></a>Jak: wymaga uwierzytelniania, aby uzyskać dostęp do tabel

Jeśli chcesz używać uwierzytelniania usługi aplikacji z punktem końcowym tabel, musisz skonfigurować uwierzytelniania aplikacji usługi w [portalu Azure] najpierw.  Aby uzyskać więcej informacji o konfigurowaniu uwierzytelniania w usłudze Azure aplikacji przejrzyj przewodnik konfiguracji dla dostawcy tożsamości, które mają być używane:

- [Jak skonfigurować uwierzytelnianie usługi Azure Active Directory]
- [Jak skonfigurować uwierzytelnianie Facebook]
- [Jak skonfigurować uwierzytelnianie Google]
- [Jak skonfigurować Authentication firmy Microsoft]
- [Jak skonfigurować uwierzytelnianie serwisu Twitter]

Każda tabela zawiera właściwości programu access, który może być używany do kontrolowania dostępu do tabeli.  Poniższy przykład pokazuje tabelę statycznie zdefiniowanych z wymagane uwierzytelnienie.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Właściwość programu access można wykonać jedną z trzech wartości

  - *anonimowe* wskazuje, że z aplikacją kliencką jest dozwolona odczytać danych bez uwierzytelniania
  - *Uwierzytelnianie* oznacza, że z aplikacją kliencką musi wysłany tokenu uwierzytelniania prawidłowych z żądaniem
  - *wyłączone* wskazuje, że w tej tabeli jest obecnie wyłączona

Jeśli właściwość programu access nie jest dostęp nie uwierzytelniony jest dozwolone.

### <a name="howto-tables-getidentity"></a>Jak: uwierzytelniania oświadczeń za pomocą tabel

Możesz skonfigurować różne oświadczeń, które są wymagane po skonfigurowaniu uwierzytelniania.  Roszczenia te nie są zwykle dostępne za pośrednictwem `context.user` obiektu.  Jednak mogą zostać pobrane przy użyciu `context.user.getIdentity()` metody.  `getIdentity()` Metoda zwraca zobowiązanie, która jest rozpoznawana jako obiektu.  Obiekt jest wprowadzić przy użyciu metody uwierzytelniania (facebook, google, twitter, microsoftaccount lub aad).

Po skonfigurowaniu uwierzytelniania Account Microsoft i żądania rościć sobie adresy e-mail można na przykład dodać adres e-mail do rekordu za pomocą sterownika poniższej tabeli:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Aby sprawdzić, jakie roszczeń są dostępne, za pomocą przeglądarki sieci web do wyświetlania `/.auth/me` punktu końcowego witryny.

### <a name="howto-tables-disabled"></a>Jak: Wyłącz dostęp do określonej tabeli operacji

Oprócz znajdujących się w tabeli, właściwość dostępu może służyć do kontrolowania poszczególnych operacji.  Istnieją cztery czynności:

  - *Przeczytaj* jest operacja RESTful UZYSKAĆ w tabeli
  - *Wstawianie* jest operacja RESTful WPIS w tabeli
  - *Aktualizowanie* jest operacja RESTful poprawki w tabeli
  - *Usuwanie* jest operację usuwania RESTful w tabeli

Możesz na przykład podanie tabeli nieuwierzytelnionych tylko do odczytu:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Jak: dopasowywanie kwerendę, która jest używana z operacjami wykonywanymi w tabeli

Typowe wymagane operacje tabeli jest zapewnienie ograniczeniami widok danych.  Na przykład może dostarczyć tabeli oznaczonej przy użyciu Identyfikatora użytkownika uwierzytelnionego, tak, aby można było tylko odczytywanie lub zaktualizować swoje rekordy.  Oferuje następujące definicja tabeli:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Operacje, które zwykle wykonania kwerendy mają właściwość kwerendy, które można dostosować przy użyciu klauzuli where w klauzuli. Właściwość kwerendy jest obiektem [QueryJS] , który umożliwia przekonwertowanie zapytania OData na coś, co może przetworzyć danych wewnętrznej bazy danych.  W przypadku prostych równości (na przykład mieszanego) można używać mapę. Możesz również dodać określonych klauzul SQL:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Jak: Konfigurowanie Usuń wygładzone w tabeli

Usuń wygładzone nie usuwa rekordy.  Zamiast tego oznaczane ich jako usunięte w bazie danych, ustawiając usuniętej kolumny na PRAWDA.  SDK aplikacje Mobile Azure usunięte wygładzone rekordy są automatycznie usuwane z wyników chyba że SDK klienta Mobile używa IncludeDeleted().  Aby skonfigurować tabelę do usunięcia wygładzone, ustaw `softDelete` właściwości w pliku definicji tabeli:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Należy ustalić mechanizm usuwanie rekordów — z aplikacją kliencką za pośrednictwem WebJob, funkcja Azure lub niestandardowe interfejsu API.

### <a name="howto-tables-seeding"></a>Jak: zapełnić bazy danych z danymi

Podczas tworzenia nowej aplikacji, możesz zapełnić tabeli z danymi.  Można to zrobić w pliku JavaScript definicja tabeli w następujący sposób:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Obsługiwanie danych jest wykonywane tylko po utworzeniu tabeli przez SDK aplikacje Mobile Azure.  Jeśli tabela już istnieje w bazie danych, bez danych zostanie dodane do tabeli.  Jeśli włączono dynamiczne schematu schematu jest wnioskowane pestkowych danych.

Zalecamy, aby jawnie połączenia `tables.initialize()` metodę w celu utworzenia tabeli podczas uruchamiania usługi.

### <a name="Swagger"></a>Jak: Włączanie obsługi Swagger

Azure usługi aplikacji Mobile zawiera wbudowane [Swagger] pomocy technicznej.  Aby włączyć obsługę Swagger, należy najpierw zainstalować interfejsu użytkownika swagger jako zależność:

    npm install --save swagger-ui

Po zainstalowaniu można włączyć obsługę Swagger w Konstruktorze Azure aplikacji Mobile:

    var mobile = azureMobileApps({ swagger: true });

Możesz prawdopodobnie tylko chcesz włączyć obsługę Swagger w wersjach rozwoju.  Można to zrobić z wykorzystaniem `NODE_ENV` ustawienie aplikacji:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Punkt końcowy swagger znajduje się w http://_yoursite_.azurewebsites.net/swagger.  Masz dostęp do Swagger interfejsu użytkownika za pomocą `/swagger/ui` punktu końcowego.  Jeśli wybierzesz opcję wymagania uwierzytelniania przez cały aplikacji, Swagger powoduje błąd.  Aby uzyskać najlepsze wyniki, wybierz pozycję, aby umożliwić nieuwierzytelnionych żądań za pośrednictwem w uwierzytelniania usługi Azure aplikacji / ustawienia pozwolenie, następnie kontrolować, za pomocą uwierzytelniania `table.access` właściwości.

Możesz również dodać opcję Swagger usługi `azureMobile.js` plik, jeśli chcesz tylko Swagger pomocy technicznej podczas tworzenia lokalnie.

## <a name="a-namepushpush-notifications"></a><a name="push">Powiadomienia wypychane

Aplikacje Mobile można zintegrować z koncentratory powiadomienie Azure, aby umożliwić wysyłanie powiadomień wypychanych docelowych miliony urządzeń na wszystkich platformach głównych. Za pomocą koncentratorów powiadomień, możesz wysłać powiadomień wypychanych w systemie iOS, Android i systemu Windows. Więcej informacji dotyczących można zrobić z koncentratorów powiadomień, zobacz [Omówienie koncentratory powiadomienie](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Jak: Wyślij powiadomienia wypychane

Poniższy kod przedstawia sposób użycia obiektu wypychanych do wysyłania powiadomień wypychanych emisji do urządzeń z systemem iOS zarejestrowanych:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Tworząc rejestracji wypychanych szablonu w kliencie, możesz zamiast tego wysłać wiadomość wypychanych szablonu do urządzeń na wszystkich obsługiwanych platformach. Poniższy kod przedstawia sposób wysłania powiadomienia szablonu:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>Jak: wysyłania powiadomień wypychanych dla użytkownika uwierzytelnionego przy użyciu tagów

Gdy żaden uwierzytelniony użytkownik rejestruje powiadomienia wypychane, znacznika identyfikator użytkownika zostaną automatycznie dodane do rejestracji. Korzystając z tego tagu, możesz wysłać powiadomienia wypychane do wszystkich urządzeń zarejestrowana przez określonego użytkownika. Poniższy kod pobiera identyfikator zabezpieczeń użytkownika zgłaszającego żądanie i wysyła powiadomienia wypychane szablonu do każdej rejestracji urządzenia dla tego użytkownika:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Podczas rejestrowania dla powiadomień wypychanych w kliencie uwierzytelnionego, upewnij się, że przed podjęciem rejestracji zakończeniem uwierzytelniania.

## <a name="CustomAPI"></a>Niestandardowe interfejsy API

###  <a name="howto-customapi-basic"></a>Jak: Definiowanie niestandardowego interfejsu API


Oprócz dostępu do danych interfejsu API za pośrednictwem punktu końcowego /tables aplikacje Mobile Azure umożliwia niestandardowy zakres interfejsu API.  Niestandardowe interfejsy API są definiowane w podobny sposób jak definicje tabel i można uzyskać dostęp do tych samych urządzeń, w tym uwierzytelniania.

Jeśli chcesz używać uwierzytelniania usługi aplikacji z interfejsem API niestandardowe, należy skonfigurować uwierzytelniania aplikacji usługi w [portalu Azure] najpierw.  Aby uzyskać więcej informacji o konfigurowaniu uwierzytelniania w usłudze Azure aplikacji przejrzyj przewodnik konfiguracji dla dostawcy tożsamości, które mają być używane:

- [Jak skonfigurować uwierzytelnianie usługi Azure Active Directory]
- [Jak skonfigurować uwierzytelnianie Facebook]
- [Jak skonfigurować uwierzytelnianie Google]
- [Jak skonfigurować Authentication firmy Microsoft]
- [Jak skonfigurować uwierzytelnianie serwisu Twitter]

Niestandardowe interfejsy API są definiowane w podobny sposób co w interfejsie API tabel.

1. Utwórz katalog **interfejsu api**
2. Utwórz plik JavaScript definicji interfejsu API w katalogu **interfejsu api** .
3. Użyj metody importu do zaimportowania katalogu **interfejsu api** .

Poniżej przedstawiono definicję interfejsu api prototypu na podstawie próbki basic aplikacji, które wcześniej użyliśmy w.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Spójrzmy na przykład interfejsu API, która zwraca datę server przy użyciu metody _Date.now()_ .  Oto plik api/date.js:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Każdy parametr jest jednym z zlecenia standardowe RESTful - GET, WPIS, poprawki lub Usuń.  Metoda jest funkcją [Pośredniczącym ExpressJS] standardowy wysyła wymagane dane wyjściowe.

### <a name="howto-customapi-auth"></a>Jak: wymaga uwierzytelniania, aby uzyskać dostęp do niestandardowego interfejsu API

Azure SDK aplikacje Mobile uwierzytelnianie jest realizowanie w ten sam sposób API niestandardowe i tabele punktu końcowego.  Aby dodać uwierzytelniania API utworzone w poprzedniej sekcji, należy dodać właściwości programu **access** :

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Można też określić uwierzytelniania na określonych operacji:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Tym samym token, który jest używany dla punktu końcowego tabel należy użyć dla niestandardowych interfejsów API wymaga uwierzytelniania.

### <a name="howto-customapi-auth"></a>Jak: obsługiwać przekazywania dużych plików

Azure SDK aplikacje Mobile używa [pośredniczącym parsera treści](https://github.com/expressjs/body-parser) , aby zaakceptować i dekodowanie treść w przesłanych elementów.  Można wstępnie skonfigurować parsera treści do przyjęcia wysyłanie plików większych:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Plik jest kodowanie przed transmisją base-64.  Zwiększa rozmiar rzeczywisty przekazywania (i w związku z tym rozmiar należy uwzględnić).

### <a name="howto-customapi-sql"></a>Jak: wykonywanie niestandardowe instrukcje SQL

SDK aplikacje Azure Mobile umożliwia dostęp do całego kontekstu za pośrednictwem obiektu żądania, co pozwala na łatwe wykonywanie parametryczne instrukcji SQL w celu dostawcy danych:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Debugowanie, łatwo tabel i łatwe interfejsy API

### <a name="howto-diagnostic-logs"></a>Jak: debugowanie, diagnozowanie i rozwiązywanie problemów z aplikacji Azure Mobile

Usługa Azure aplikacji udostępnia kilka debugowania i techniki dla aplikacji Node.js rozwiązywania problemów.
Należy zapoznać się z następującymi artykułami, aby rozpocząć rozwiązywanie problemów z komórkowym Node.js wewnętrznej bazy danych:

- [Monitorowanie Azure usługi aplikacji]
- [Włącz rejestrowanie diagnostyczne w usłudze Azure aplikacji]
- [Rozwiązywanie problemów z usługą Azure aplikacji w programie Visual Studio]

Aplikacje node.js mają dostęp do szeroką gamę narzędzi diagnostycznych dziennika.  Wewnętrznie SDK Node.js aplikacje Mobile Azure używa [Winston] rejestrowanie diagnostyczne.  Rejestrowanie jest włączany automatycznie po włączeniu trybu debugowania lub ustawiając ustawienie aplikacji **MS_DebugMode** na Prawda w [Azure portal]. Dzienniki wygenerowane są wyświetlane w dziennikach diagnostycznych w [Azure portal].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Jak: Praca z tabelami łatwe w portalu Azure

Łatwe tabel w portalu umożliwiają tworzenie folderów i Praca z tabelami bezpośrednio w portalu. Można nawet edytować operacje tabeli przy użyciu edytora usługi aplikacji.

Po kliknięciu **łatwe tabel** w ustawieniach witryny wewnętrznej bazy danych, możesz Dodawanie, modyfikowanie lub usuwanie tabeli. Można też wyświetlić dane w tabeli.

![Praca z tabelami łatwe](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Następujące polecenia są dostępne na pasku poleceń dla tabeli:

+ **Zmień uprawnienia** - modyfikowanie uprawnień do odczytu, wstawianie, aktualizowanie i usuwanie operacje w tabeli. 
  Aby umożliwić dostęp anonimowy, aby wymagać uwierzytelniania lub wyłączyć dostęp do tej operacji są opcje. 
+ **Edytowanie skryptu** - plik skryptu dla tabeli zostanie otwarty w edytorze usługi aplikacji.
+ **Zarządzanie schematem** - Dodawanie lub usuwanie kolumn lub zmienić indeks tabeli.
+ **Wyczyść tabelę** — obcina istniejącej tabeli jest usunięcie wszystkich wierszy danych, ale opuszczania schematu bez zmian.
+ **Usuwanie wierszy** — usuwanie pojedynczych wierszy danych.
+ **Widok streaming dzienniki** — łączy do przesyłanie strumieniowe usługi rejestrowania dla witryny.

###<a name="work-easy-apis"></a>Jak: Praca z API łatwe w portalu Azure

Łatwe interfejsy API w portalu umożliwiają tworzenie folderów i Praca z niestandardowe interfejsy API bezpośrednio w portalu. Możesz edytować skrypty interfejsu API przy użyciu edytora usługi aplikacji.

Po kliknięciu **API łatwe** w ustawieniach witryny wewnętrznej bazy danych, możesz Dodawanie, modyfikowanie lub usuwanie niestandardowego końcowego interfejsu API.

![Praca z łatwe interfejsy API](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

W portalu można Zmienianie uprawnień dostępu dla danego działania HTTP, edytować plik skryptu interfejsu API w edytorze usługi aplikacji lub wyświetlanie dzienników przesyłanie strumieniowe.

###<a name="online-editor"></a>Jak: edytowanie kodu w edytorze usługi aplikacji

Azure portal umożliwia edytowanie plików skryptów Node.js wewnętrznej bazy danych w edytorze usługi aplikacji bez konieczności pobierania projektu na komputer lokalny. Aby edytować pliki skryptów w edytorze online:

1. W swojej aplikacji Mobile karta wewnętrznej bazy danych, kliknij polecenie **wszystkie ustawienia** > **łatwe tabel** lub **Łatwe interfejsy API**kliknij tabelę lub interfejsu API, a następnie kliknij przycisk **Edytuj skrypt**. Plik skryptu zostanie otwarty w edytorze usługi aplikacji.

    ![Edytor usług aplikacji](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Wprowadź zmiany w pliku kod w edytorze online. Zmiany są zapisywane automatycznie podczas pisania.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Szybki Start android klienta]: app-service-mobile-android-get-started.md
[Szybki Start klienta Cordova Apache]: app-service-mobile-cordova-get-started.md
[iOS klienta Szybki Start]: app-service-mobile-ios-get-started.md
[Szybki Start Xamarin.iOS klienta]: app-service-mobile-xamarin-ios-get-started.md
[Szybki Start Xamarin.Android klienta]: app-service-mobile-xamarin-android-get-started.md
[Szybki Start Xamarin.Forms klienta]: app-service-mobile-xamarin-forms-get-started.md
[Szybki Start klienta ze Sklepu Windows]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[Synchronizowanie danych w trybie offline]: app-service-mobile-offline-data-sync.md
[Jak skonfigurować uwierzytelnianie usługi Azure Active Directory]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Jak skonfigurować uwierzytelnianie Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Jak skonfigurować uwierzytelnianie Google]: app-service-mobile-how-to-configure-google-authentication.md
[Jak skonfigurować Authentication firmy Microsoft]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak skonfigurować uwierzytelnianie serwisu Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md
[Podręcznik wdrażania usługi Azure aplikacji]: ../app-service-web/web-sites-deploy.md
[Monitorowanie Azure usługi aplikacji]: ../app-service-web/web-sites-monitor.md
[Włącz rejestrowanie diagnostyczne w usłudze Azure aplikacji]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Rozwiązywanie problemów z usługą Azure aplikacji w programie Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Określanie wersji węzeł]: ../nodejs-specify-node-version-azure-apps.md
[za pomocą węzła modułów]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Planowanie zapasów]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[Przykładowe basicapp na GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[Przykładowe zadania na GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[Przykłady katalogu na GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Narzędzia node.js 1.1 programu Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[Pakiet Node.js MSSQL]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS pośredniczącym]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
