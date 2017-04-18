<properties
    pageTitle="Jak używać JavaScript SDK do Azure aplikacji dla urządzeń przenośnych"
    description="Jak v do użycia w przypadku aplikacji Mobile Azure"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-javascript-client-library-for-azure-mobile-apps"></a>Jak używać biblioteki klienta JavaScript dla Azure aplikacji dla urządzeń przenośnych

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ten przewodnik przedstawiono wykonywanie typowych scenariuszy przy użyciu najnowszych [SDK JavaScript dla aplikacji Mobile Azure]. Jeśli jesteś nowym użytkownikiem aplikacji Mobile Azure, wykonaj najpierw [Azure Mobile aplikacje Szybki Start] , aby utworzyć wewnętrznej bazie danych, a następnie utwórz tabelę. W tym przewodniku możemy skupić się na przy użyciu urządzeń przenośnych wewnętrznej bazy danych w aplikacjach sieci Web HTML i JavaScript.

## <a name="supported-platforms"></a>Platformy obsługiwane

Możemy ograniczyć Obsługa przeglądarek przez aplikacje do bieżącej i ostatni wersji głównych przeglądarek: Google Chrome, Microsoft Edge, programu Microsoft Internet Explorer i Mozilla Firefox.  Oczekujemy SDK funkcji dowolnej nowoczesne stosunkowo przeglądarki.

Pakiet jest rozpowszechniane jako moduł uniwersalny JavaScript, dzięki czemu obsługuje globals, AMD, i CommonJS formatowania.

##<a name="Setup"></a>Instalacja i wymagania wstępne

Ten przewodnik przyjęto założenie, że utworzono wewnętrznej bazie danych z tabeli. Ten przewodnik przyjęto założenie, że tabela zawiera ten sam schemat jako tabele w tych samouczkach.

Instalowanie Azure Mobile aplikacje JavaScript SDK można przeprowadzić za pośrednictwem `npm` polecenie:

```
npm install azure-mobile-apps-client --save
```

Po zainstalowaniu biblioteki znajduje się w `node_modules/azure-mobile-apps-client/dist/MobileServices.Web.min.js`.  Skopiuj ten plik do obszaru sieci web.

```
<script src="path/to/MobileServices.Web.min.js"></script>
```

Można także biblioteki jako moduł ES2015 w środowiskach CommonJS, takich jak Browserify i Webpack i jako biblioteki AMD.  Na przykład:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

[AZURE.INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

##<a name="auth"></a>Jak: uwierzytelniania użytkowników

Azure aplikacji usługi obsługuje uwierzytelnianie i autoryzowanie użytkowników aplikacji przy użyciu różnych dostawcy tożsamości zewnętrzne: Facebook, Google, Account Microsoft i Twitter. Można ustawić uprawnienia w tabelach w celu ograniczenia dostępu dla określonych operacje, aby tylko uwierzytelnieni użytkownicy. Za pomocą tożsamości uwierzytelnionych użytkowników do wykonania reguł autoryzacji w skrypty serwera. Aby uzyskać więcej informacji zobacz samouczek [Wprowadzenie do uwierzytelniania] .

Obsługiwane są dwie przebiegu uwierzytelniania: serwer i przepływ klienta.  Przepływ server zapewnia najłatwiejszym obsługę uwierzytelniania, zależy od interfejs uwierzytelniania sieci web dostawcy. Przepływ klienta umożliwia lepsza integracja możliwości specyficzne dla urządzenia z takich jak-jednokrotnej jako zależy od SDK specyficzne dla dostawcy.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Jak: Konfigurowanie urządzeń przenośnych usługi aplikacji dla zewnętrznego przekierowywania adresów URL.

Kilka typów aplikacji JavaScript za pomocą funkcji sprzężenia zwrotnego obsługiwać przepływy OAuth interfejsu użytkownika.  Te funkcje obejmują:

* Usługą lokalnie
* Załaduj ponownie Live za pomocą jonowa Framework
* Przekierowywanie do aplikacji usługi uwierzytelniania. 

Uruchomienie lokalnie może spowodować problemy, ponieważ domyślnie uwierzytelniania aplikacji usługi jest skonfigurowane tylko zezwolić na dostęp do wewnętrznej bazy danych aplikacji Mobile. Aby zmienić ustawienia aplikacji usługi, aby włączyć uwierzytelnianie podczas uruchamiania serwera lokalnie, wykonaj następujące czynności:

1. Zaloguj się do [portalu Azure]
2. Przejdź do swojej aplikacji Mobile wewnętrznej bazy danych.
3. Wybierz pozycję **Eksplorator zasobu** w menu **Narzędzia rozwoju** .
4. Kliknij przycisk **Przejdź** , aby otworzyć Eksploratora zasobów dla swojego wewnętrznej bazy danych aplikacji Mobile w nowej karcie lub w oknie.
5. Rozwiń węzeł **Konfiguracja** > **authsettings** węzeł aplikacji.
6. Kliknij przycisk **Edytuj** , aby umożliwić edytowanie zasobu.
7. Znajdź element **allowedExternalRedirectUrls** , który powinien mieć wartości null. Dodawanie usługi adresy URL w tablicy:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Zamienianie adresów URL w tablicy przy użyciu adresu URL usługi, czyli w tym przykładzie `http://localhost:3000` dla lokalnej usługi przykładowe Node.js. Można także użyć `http://localhost:4400` usługi wpływu lub innych URL, w zależności od sposobu skonfigurowania aplikacji.

8. W górnej części strony kliknij pozycję **Odczytu/zapisu**, a następnie kliknij **umieszczenie** , aby zapisać zmiany.

Należy dodać tych samych adresów URL sprzężenia zwrotnego do ustawień listy sprawdzonej CORS:

1. Przejść z powrotem do [Azure portal].
2. Przejdź do swojej aplikacji Mobile wewnętrznej bazy danych.
3. Kliknij menu **interfejsu API** **CORS** .
4. Wprowadź każdego adresu URL w polu tekstowym **Dozwolonych miejsc pochodzenia** puste.  Powoduje utworzenie nowego pola tekstowego.
5. Kliknij przycisk **ZAPISZ**
    
Po aktualizacji wewnętrznej bazy danych, można korzystać z nowych sprzężenia zwrotnego adresów URL w aplikacji.

<!-- URLs. -->
[Azure urządzeń przenośnych aplikacji Szybki Start]: app-service-mobile-cordova-get-started.md
[Wprowadzenie do uwierzytelniania]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Azure portal]: https://portal.azure.com/
[Zestaw SDK JavaScript dla Azure aplikacji dla urządzeń przenośnych]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

