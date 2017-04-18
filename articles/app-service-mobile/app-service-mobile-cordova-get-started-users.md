<properties
    pageTitle="Dodawanie uwierzytelniania na Cordova Apache przy użyciu aplikacji Mobile | Azure aplikacji usługi"
    description="Dowiedz się, jak używać aplikacji Mobile w usłudze Azure aplikacji do uwierzytelniania użytkowników aplikacji Apache Cordova przy użyciu różnych dostawców tożsamości, w tym Google, Facebook, Twitter i Microsoft."
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-apache-cordova-app"></a>Dodawanie uwierzytelniania do aplikacji Apache Cordova

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
    
## <a name="summary"></a>Podsumowanie

W tym samouczku możesz dodać do listy zadań projektu Szybki Start na Cordova Apache za pośrednictwem dostawcy tożsamości obsługiwane uwierzytelniania. Ten samouczek jest oparty na samouczek [Wprowadzenie do aplikacji Mobile] , które trzeba wykonać najpierw.

##<a name="register"></a>Zarejestruj się w aplikacji dla uwierzytelniania i konfigurowanie aplikacji usługi

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Obejrzyj klip wideo przedstawiający podobne kroki](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

##<a name="permissions"></a>Ograniczanie uprawnień do uwierzytelnionych użytkowników

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Teraz można sprawdzić, wyłączono dostęp anonimowy do swojej wewnętrznej bazy danych. W programie Visual Studio Otwórz projekt utworzony po wykonaniu samouczek [Wprowadzenie do aplikacji Mobile], a następnie uruchomienie aplikacji w **Emulatora Android Google** i sprawdź, czy nieoczekiwany błąd połączenia jest wyświetlana po uruchomieniu aplikacji.

Następnie zostanie zaktualizowany aplikacji do uwierzytelniania użytkowników przed żądaniem zasobów w aplikacji Mobile wewnętrznej bazy danych.

##<a name="add-authentication"></a>Dodawanie uwierzytelniania aplikacji

1. Otwórz projekt w **Programie Visual Studio**, a następnie otwórz `www/index.html` pliku do edycji.

2. Znajdź `Content-Security-Policy` tag meta w sekcji głowy.  Należy dodać hosta OAuth do listy dozwolonych źródeł.

  	| Dostawcy               | Nazwa dostawcy SDK | OAuth hosta                  |
  	| :--------------------- | :---------------- | :-------------------------- |
  	| Azure Active Directory | AAD               | https://login.Windows.NET   |
  	| Facebook               | serwisu Facebook          | https://www.Facebook.com    |
  	| Google                 | Google            | https://accounts.Google.com |
  	| Microsoft              | MicrosoftAccount  | https://login.Live.com      |
  	| Twitter                | Twitter           | https://API.twitter.com     |

    Przykład zawartości — — zasady zabezpieczeń (dla usługi Azure Active Directory zrealizowane) jest następująca:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.windows.net https://yourapp.azurewebsites.net; style-src 'self'">

    Należy zastąpić `https://login.windows.net` hosta OAuth w powyższej tabeli.  Aby uzyskać więcej informacji o tym tag meta, zapoznaj się z [dokumentacją zasady dotyczące zabezpieczeń zawartości] .

    Należy zauważyć, że niektórych dostawców uwierzytelniania wymaga zasady dotyczące zabezpieczeń zawartość zmienia się po używane na urządzeniach przenośnych, odpowiednie.  Na przykład bez zmian zawartości zasady dotyczące zabezpieczeń są wymagane, używając uwierzytelniania Google na urządzeniu z systemem Android.

3. Otwórz `www/js/index.js` pliku do edycji, Znajdź `onDeviceReady()` metody, i w obszarze Tworzenie klienta kod Dodaj następujące czynności:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Należy zauważyć, że ten kod zastąpić istniejący kod, który tworzy odwołanie do tabeli oraz odświeża interfejsu użytkownika.

    Metoda: login() uruchamia uwierzytelniania z dostawcą. Metoda: login() jest asynchronicznych funkcji, która zwraca zobowiązanie JavaScript.  Pozostałe inicjowania jest umieszczony w odpowiedzi zobowiązanie, tak aby nie jest wykonywana, dopóki nie zakończy metody: login().

4. W kodzie dodanego Zamień `SDK_Provider_Name` z nazwą swojego dostawcy logowania. Na przykład dla usługi Azure Active Directory, należy użyć `client.login('aad')`.

4. Uruchom projektu.  Po zakończeniu projektu inicjowania aplikacji pojawi się strona logowania OAuth dla dostawcy wybranym uwierzytelniania.

##<a name="next-steps"></a>Następne kroki

* Dowiedz się więcej [O uwierzytelnianie] za pomocą usługi aplikacji Azure.
* Kontynuuj samouczka, dodając [Powiadomienia Push] do aplikacji Apache Cordova.

Dowiedz się, jak używać SDK.

* [Apache Cordova SDK]
* [Serwer programu ASP.NET SDK]
* [Serwer node.js SDK]

<!-- URLs. -->
[Wprowadzenie do aplikacji Mobile]: app-service-mobile-cordova-get-started.md
[Dokumentacja zasady dotyczące zabezpieczeń zawartości]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Powiadomienia wypychane]: app-service-mobile-cordova-get-started-push.md
[Uwierzytelnianie — informacje]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md 
[Serwer programu ASP.NET SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Serwer node.js SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
