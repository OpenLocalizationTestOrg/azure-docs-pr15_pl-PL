<properties
    pageTitle="Uwierzytelnianie użytkownika w przypadku aplikacji interfejsu API usługi aplikacji Azure | Microsoft Azure"
    description="Dowiedz się, jak chronić aplikacji interfejsu API usługi aplikacji Azure zezwolenia na dostęp tylko do uwierzytelnionych użytkowników."
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
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Uwierzytelnianie użytkownika w przypadku aplikacji interfejsu API usługi aplikacji Azure

## <a name="overview"></a>Omówienie

W tym artykule pokazano, jak chronić aplikacji interfejsu API Azure, tak aby tylko uwierzytelnieni użytkownicy mogą Wywołaj go. Tego artykułu przyjęto założenie, należy przeczytać [Omówienie uwierzytelniania Azure aplikacji usługi](../app-service/app-service-authentication-overview.md).

Opisano następujące zagadnienia:

* Jak skonfigurować dostawcę uwierzytelniania ze szczegółami dla usługi Azure Active Directory (Azure AD).
* Jak korzystać chronionego aplikacji interfejsu API przy użyciu [Active Directory uwierzytelniania biblioteki (ADAL) dla języka JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

Ten artykuł zawiera dwie sekcje:

* Sekcja [sposobu konfigurowania uwierzytelniania użytkowników w usłudze Azure aplikacji](#authconfig) ogólnie opisano sposób konfigurowania uwierzytelniania użytkowników dla dowolnej aplikacji interfejsu API i jednakowo dotyczy wszystkich ram obsługiwane przez usługi aplikacji, w tym .NET, Node.js i Java.

* Zaczynając od [kontynuowaniem samouczki aplikacje interfejsu API .NET](#tutorialstart) sekcji prowadnice artykuł podczas konfigurowania aplikacji przykładowej za pomocą .NET wewnętrzna baza danych i z przodu AngularJS zakończenia tak, aby korzystała usługi Azure Active Directory do uwierzytelniania użytkowników. 

## <a id="authconfig"></a>Jak skonfigurować uwierzytelnianie użytkownika w usłudze Azure aplikacji

Ta sekcja zawiera ogólne instrukcje, które dotyczą dowolnej aplikacji interfejsu API. Aby uzyskać procedury określonej aplikacji przykładowej do .NET listy wykonaj przejdź do [kontynuowaniem samouczki .NET — wprowadzenie](#tutorialstart).

1. W [portalu Azure](https://portal.azure.com/), przejdź do karta **Ustawienia** aplikacji interfejsu API, którą chcesz chronić, Znajdź sekcję **Funkcje** , a następnie kliknij **uwierzytelniania i autoryzacji**.

    ![Portal Azure uwierzytelniania i autoryzacji](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. W **uwierzytelniania i autoryzacji** karta, **Wybierz polecenie**.

4. Wybierz jedną z opcji z listy rozwijanej **akcję do wykonania po żądanie nie jest uwierzytelniony** .

    * Jeśli chcesz, aby tylko uwierzytelnieni połączenia do osiągnięcia aplikacji interfejsu API, wybierz jedną z opcji **logowania przy użyciu...** . Ta opcja umożliwia ochronę aplikacji interfejsu API bez zapisywania cały kod, który jest uruchamiany w nim.

    * Jeśli chcesz, aby wszystkie połączenia do osiągnięcia aplikacji interfejsu API, wybierz pozycję **Zezwalaj na żądanie (żadnej akcji)**. Ta opcja umożliwia bezpośrednie nieuwierzytelnionych osób dzwoniących do wyboru dostawców uwierzytelniania. Po wybraniu tej opcji trzeba napisać kod obsługujący autoryzacji.

    Aby uzyskać więcej informacji zobacz [Uwierzytelnianie i autoryzacja interfejsu API aplikacji w usłudze Azure aplikacji](app-service-api-authentication.md#multiple-protection-options).

5. Zaznacz jeden lub więcej **Dostawców uwierzytelniania**.

    Rysunek przedstawia opcje, które wymagają wszystkich osób dzwoniących uwierzytelnianie przez Azure AD.
 
    ![Azure portalu karta uwierzytelniania i autoryzacji](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    Po wybraniu dostawcę uwierzytelniania portalu jest wyświetlana karta konfiguracji dla tego dostawcy. 

    Aby uzyskać szczegółowe instrukcje, których wyjaśniono, jak skonfigurować karty Konfiguracja dostawcy uwierzytelniania Zobacz [jak skonfigurować aplikację aplikacji usługi, aby użyć logowania usługi Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (Łącze prowadzące do artykułu o Azure AD, ale samym artykule zawiera karty, zawierających łącza do artykułów dotyczących innych dostawców uwierzytelniania).

7. Po wykonaniu tych Karta Konfiguracja dostawcy uwierzytelniania, kliknij **przycisk OK**.

7. W **uwierzytelniania i autoryzacji** karta, kliknij przycisk **Zapisz**.

Gdy jest to możliwe, aplikacji usługi uwierzytelnia wszystkie interfejsu API przed osiągnięcia aplikacji interfejsu API. Usług uwierzytelniania działa tak samo dla wszystkich języków obsługiwanych przez usługi aplikacji, takich jak .NET, Node.js i Java. 

Aby uwierzytelnieni interfejsu API, wywołujący zawiera token okaziciela OAuth 2.0 Dostawca uwierzytelniania w nagłówku autoryzacji żądania HTTP. Token można uzyskać przy użyciu zestawu SDK dostawcy uwierzytelniania.

## <a id="tutorialstart"></a>Kontynuowanie samouczki aplikacje interfejsu API .NET

Jeśli używasz Node.js lub Java samouczki dotyczące aplikacji interfejsu API, przejdź do następnego artykuł [usługi kapitału uwierzytelniania dla aplikacji interfejsu API](app-service-api-dotnet-service-principal-auth.md). 

Jeśli po samouczka .NET dla aplikacji interfejsu API i zostały już rozmieszczone przykładowej aplikacji zgodnie ze wskazówkami w samouczkach [pierwszą](app-service-api-dotnet-get-started.md) i [drugą](app-service-api-cors-consume-javascript.md) , przejdź do sekcji [Konfigurowanie uwierzytelniania w aplikacji usługi i Azure AD](#azureauth) .

Jeśli chcesz skorzystać z tego samouczka bez pośrednictwa samouczki pierwszą i drugą, wykonaj następujące czynności, w których wyjaśniono, jak rozpocząć pracę przy użyciu zautomatyzowany proces wdrażania aplikacji przykładowej.

>[AZURE.NOTE] Poniższe kroki pomogą Ci do tego samego punktu początkowego tak, jakby czy pierwszych dwóch samouczki, z jednym wyjątkiem: Visual Studio nie wiesz już, które w przeglądarce lub aplikację interfejsu API, która pobiera wdrożony każdego projektu. Oznacza to, że nie będzie można uzyskać dokładne instrukcje w tym samouczku, których wyjaśniono, jak wdrożyć do prawej elementów docelowych. Jeśli nie masz doświadczenia w ważniejszymi sposobów wykonywania kroków wdrażania samodzielnie, warto obserwować samouczka w [pierwszym samouczek](app-service-api-dotnet-get-started.md) niż rozpocząć ten proces automatycznego wdrażania.

1. Upewnij się, że wszystkie wymagania wstępne wymienione w [pierwszym samouczek](app-service-api-dotnet-get-started.md). Oprócz wymagania wstępne wymienione tych samouczkach uwierzytelniania założono pracowano przy użyciu aplikacji sieci web aplikacji usługi i aplikacje interfejsu API programu Visual Studio i Azure portal.

2. Kliknij przycisk **Deploy Azure** w [pliku readme repozytorium Przykładowa lista zadań do wykonania](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) do wdrażania aplikacji interfejsu API i aplikacji sieci web. Zanotuj grupa zasobów Azure, który zostanie utworzony, jak można to później, aby wyszukać aplikacji sieci web i nazwy aplikacji interfejsu API.
 
3. Pobierz lub klonowanie [repozytorium Przykładowa lista zadań do wykonania](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) , aby uzyskać kod lokalnie będą współdziałać z w programie Visual Studio.

## <a id="azureauth"></a>Konfigurowanie uwierzytelniania w aplikacji usługi i Azure AD

Teraz masz aplikacji działa usługa Azure aplikacji bez wymaga uwierzytelnienia użytkowników. W tej sekcji można dodać uwierzytelniania, wykonując następujące czynności:

* Konfigurowanie aplikacji usługi do wymagania uwierzytelniania usługi Azure Active Directory (Azure AD) do nawiązywania połączeń z aplikacji warstwa środkowa interfejsu API.
* Tworzenie aplikacji Azure AD.
* Konfigurowanie aplikacji Azure AD wysyłanie token okaziciela po zalogowaniu do fronton AngularJS. 

Jeśli napotkasz problemy podczas zgodnie z instrukcjami samouczka, zobacz sekcję [Rozwiązywanie problemów](#troubleshooting) na koniec samouczka. 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Konfigurowanie uwierzytelniania dla aplikacji interfejsu API warstwy środkowej

1. W [portalu Azure](https://portal.azure.com/), przejdź do karta **Ustawienia** aplikacji interfejsu API utworzonego dla projektu ToDoListAPI, Znajdź sekcję **Funkcje** , a następnie kliknij **uwierzytelniania i autoryzacji**.

    ![Portal Azure uwierzytelniania i autoryzacji](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. W **uwierzytelniania i autoryzacji** karta, **Wybierz polecenie**.

4. Na liście rozwijanej **akcję do wykonania po żądanie nie jest uwierzytelniony** wybierz pozycję **Zaloguj się przy użyciu usługi Azure Active Directory**.

    Ta opcja zapewnia, że żadne żądania unauathenticated górny aplikacji interfejsu API. 

5. W obszarze **Dostawców uwierzytelniania**kliknij **Usługi Azure Active Directory**.

    ![Azure portalu karta uwierzytelniania i autoryzacji](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. W karta **Ustawienia usługi Azure Active Directory** kliknij przycisk **Express**

    ![Opcja Express uwierzytelniania i autoryzacji portal Azure](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    Z opcją **Express** aplikacji usługi można automatycznie utworzyć aplikację Azure AD w Azure AD [dzierżawy](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Nie musisz utworzyć dzierżawy, ponieważ każdy konto Azure istnieje automatycznie.

7. W obszarze **tryb zarządzania**kliknij pozycję **Utwórz nową aplikację AD** , jeśli nie została jeszcze wybrana i zwróć uwagę, wartość, która znajduje się w polu tekstowym **Tworzenie aplikacji** ; wyszukiwanie będzie tej AAD aplikacji w portalu klasyczny Azure później.

    ![Ustawienia Azure portal Azure AD](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure automatycznie utworzy aplikacji Azure AD o nazwie wskazanej w dzierżawie usługi Azure AD. Domyślnie aplikacja Azure AD nosi nazwę taka sama, jak aplikacja interfejsu API. Jeśli wolisz, należy wprowadzić pod inną nazwą.
 
7. Kliknij **przycisk OK**.

7. W **uwierzytelniania i autoryzacji** karta, kliknij przycisk **Zapisz**.

    ![Kliknij przycisk Zapisz](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Teraz tylko użytkowników w dzierżawie Azure AD można zadzwonić aplikacji interfejsu API.

### <a name="optional-test-the-api-app"></a>Opcjonalnie: Testowanie aplikacji interfejsu API

1. W przeglądarce przejdź do adresu URL aplikacji interfejsu API: karta **aplikacji interfejsu API** w portalu Azure, kliknij łącze pod **adresem URL**.  

    Nastąpi przekierowanie do ekranu logowania, ponieważ nieuwierzytelnionych żądania nie są dozwolone do osiągnięcia aplikacji interfejsu API.

    Jeśli przeglądarka przechodzi na stronę "utworzono", może już zarejestrowane przeglądarce--w takim przypadku Otwórz okno InPrivate lub Incognito i przejdź do adresu URL aplikacji interfejsu API.

2. Zaloguj się przy użyciu poświadczeń dla użytkownika w dzierżawie usługi Azure AD.

    Gdy użytkownik jest zalogowany, "utworzono" strona jest wyświetlana w przeglądarce.

9. Zamknij przeglądarkę.

### <a name="configure-the-azure-ad-application"></a>Konfigurowanie aplikacji Azure AD

Podczas konfigurowania Azure AD uwierzytelniania aplikacji usługi tworzona aplikacja Azure AD. Domyślnie nowe Azure AD aplikacji został skonfigurowany do dostarczania token okaziciela do adresu URL aplikacji interfejsu API. W tej sekcji można skonfigurować aplikację Azure AD zapewnienie token okaziciela w celu AngularJS przedniej zamiast bezpośrednio do aplikacji warstwy środkowej interfejsu API. Fronton AngularJS wyśle token do aplikacji interfejsu API, gdy wywołuje aplikacji interfejsu API.

>[AZURE.NOTE] Zachować proces jako proste, jak to możliwe, ten samouczek zastosowania w jednym Azure AD aplikacji zarówno fronton i do środka poziomu interfejsu API aplikacji. Innym rozwiązaniem jest użycie dwóch aplikacji Azure AD. W takim przypadku będą musiały nadać uprawnienie aplikacji Azure AD fronton połączenie aplikacji Azure AD inicjał drugiego poziomu. Tej metody wiele aplikacji czy zapewniają większą kontrolę nad uprawnienia do każdego poziomu.

11. W [portalu klasyczny Azure](https://manage.windowsazure.com/)przejdź do **Usługi Azure Active Directory**.

    Należy korzystać z portalu klasyczny, ponieważ niektóre ustawienia usługi Azure Active Directory, które muszą uzyskiwać dostęp do nie są jeszcze dostępne w bieżącym portal Azure.

12. Na karcie **katalog** kliknij dzierżawy usługi AAD.

    ![Azure AD w klasycznym portalu](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Kliknij pozycję **Aplikacje > aplikacje właścicielem mojej firmy**, a następnie kliknij znacznik wyboru.

    Możesz również może być konieczne odświeżenie strony w celu wyświetlenia nowej aplikacji.

15. Na liście aplikacji kliknij nazwę, w którym Azure tworzony po włączeniu uwierzytelniania dla aplikacji interfejsu API.

    ![Karta Azure AD aplikacji](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Kliknij przycisk **Konfiguruj**.

    ![Karta Azure AD Konfiguruj](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. Ustawianie **logowania jednokrotnego adresu URL** do adresu URL dla AngularJS aplikacji sieci web, nie ukośnika.

    Na przykład: https://todolistangular.azurewebsites.net

    ![Adres URL logowania jednokrotnego](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. Ustaw **Adres URL odpowiedź** do adresu URL dla aplikacji sieci web, a nie ukośnika.

    Na przykład: https://todolistsangular.azurewebsites.net

16. Kliknij przycisk **Zapisz**.

    ![Adres URL odpowiedzi](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. U dołu strony kliknij **manifest Zarządzaj > Pobierz manifest**.

    ![Pobieranie manifestu](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Pobierz plik do lokalizacji, w którym można go edytować.

16. Pobrany plik manifestu, wyszukiwanie `oauth2AllowImplicitFlow` właściwości. Zmienianie wartości tej właściwości z `false` do `true`, a następnie zapisz plik.

    To ustawienie jest wymagane do uzyskania dostępu z aplikacji jednostronicowy JavaScript. Umożliwia token okaziciela Oauth 2.0 w fragment adresu URL.

16. Kliknij pozycję **manifest Zarządzaj > Przekaż manifest**i przekaż plik zaktualizowany w poprzednim kroku.

    ![Przekazywanie manifest](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. Skopiuj wartość **Identyfikator klienta** i zapisać go w innym miejscu, który można pobrać ze później.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Konfigurowanie projektu ToDoListAngular uwierzytelniania

W tej sekcji, możesz zmienić fronton AngularJS tak, aby korzystała Active Directory uwierzytelniania biblioteki (ADAL) dla JS umożliwia uzyskanie token okaziciela dla zalogowany użytkownik z usługi Azure Active Directory. Kod będzie obejmować token żądania HTTP wysyłane do warstwy środkowej, jak pokazano na poniższym rysunku. 

![Diagram uwierzytelnianie użytkownika](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Wprowadzić następujące zmiany do plików w programie project ToDoListAngular.

1. Otwórz plik *index.html* .

2. Usuń komentarze wiersze zawierające odwołanie Active Directory uwierzytelniania biblioteki (ADAL) JS skryptów.

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. Otwórz plik *app/scripts/app.js* .

2. Komentarz blok kodu oznaczone uwierzytelniania"bez" i Usuń komentarze blok kodu oznaczone uwierzytelniania"z".

    Ta zmiana odwołuje się do dostawcy uwierzytelniania ADAL JS i zawiera wartości konfiguracji do niego. W następującej procedurze można ustawić wartości konfiguracji aplikacji interfejsu API i aplikacji Azure AD.

8. Kod, który ustawia `endpoints` zmienną, ustawić adres URL interfejsu API adres URL aplikacji interfejsu API utworzone dla projektu ToDoListAPI i Ustaw identyfikator aplikacji Azure AD identyfikator klienta, skopiowany z portalu klasyczny Azure.

    Kod teraz jest podobny do następującego przykładu.

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. W wywołaniu `adalProvider.init`, ustaw `tenant` do nazwy dzierżawy i `clientId` na samą wartość użyto w poprzednim kroku.

    Kod teraz jest podobny do następującego przykładu.

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    Te zmiany we `app.js` określić, że kod wywołujący oraz o nazwie interfejsu API są w tej samej aplikacji Azure AD.

1. Otwórz plik *app/scripts/homeCtrl.js* .

2. Komentarz blok kodu oznaczone uwierzytelniania"bez" i Usuń komentarze blok kodu oznaczone uwierzytelniania"z".

1. Otwórz plik *app/scripts/indexCtrl.js* .

2. Komentarz blok kodu oznaczone uwierzytelniania"bez" i Usuń komentarze blok kodu oznaczone uwierzytelniania"z".

### <a name="deploy-the-todolistangular-project-to-azure"></a>Wdrażanie projektu ToDoListAngular Azure

8. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy ToDoListAngular projektu, a następnie kliknij **Publikuj**.

9. Kliknij przycisk **Publikuj**.

    Program Visual Studio wdraża projektu i otwarcie przeglądarki podstawowy adres URL aplikacji sieci web. To zostanie wyświetlona strony błędu 403, która jest normalnym próba przejdź do adresu URL podstawowy interfejs API sieci Web w przeglądarce.

    Nadal masz zmiany do aplikacji interfejsu API warstwa środkowa przed można przetestować aplikację.

10. Zamknij przeglądarkę.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Konfigurowanie projektu ToDoListAPI uwierzytelniania

Obecnie wysyła projektu ToDoListAPI "*" jako `owner` wartość ToDoListDataAPI. W tej sekcji, możesz zmienić kod, aby wysłać identyfikator logowania użytkownika.

Należy wprowadzić następujące zmiany w projekcie ToDoListAPI.

1. Otwórz plik *Controllers/ToDoListController.cs* i Usuń komentarze wiersza w każdej metody akcji, który ustawia `owner` do Azure AD `NameIdentifier` rościć sobie wartość. Na przykład:

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **Ważne**: nie Usuń komentarze z kodem w `ToDoListDataAPI` metody; można będzie to zrobić później samouczek kapitału uwierzytelniania usługi.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Wdrażanie projektu ToDoListAPI Azure

8. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy ToDoListAPI projektu, a następnie kliknij **Publikuj**.

9. Kliknij przycisk **Publikuj**.

    Program Visual Studio wdraża projektu i otwarcie przeglądarki podstawowy adres URL aplikacji interfejsu API.

10. Zamknij przeglądarkę.

### <a name="test-the-application"></a>Przetestuj aplikację

9. Przejdź do adresu URL aplikacji sieci web **przy użyciu protokołu HTTPS, nie HTTP**.

8. Kliknij kartę **Lista zadań do wykonania** .

    Zostanie wyświetlony monit, aby się zalogować.

9. Zaloguj się przy użyciu poświadczeń użytkownika w dzierżawie AAD.

10. Zostanie wyświetlona strona **Listy zadań do wykonania** .

    ![Lista zadań do wykonania strony](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    Nie elementów do wykonania są wyświetlane, ponieważ do tej pory wszystkie zostały właściciela "*". Teraz warstwy środkowej żąda elementów zalogowany użytkownik, a nie utworzono jeszcze.

11. Dodawanie nowych elementów do wykonania, aby sprawdzić, czy aplikacja działa.

12. W innym oknie przeglądarki, przejdź do adresu URL interfejsu użytkownika Swagger interfejsu API ToDoListDataAPI aplikacji, a następnie kliknij pozycję **listy zadań > Uzyskaj**. Wprowadź gwiazdkę dla `owner` parametr, a następnie kliknij przycisk **Spróbuj go**.

    Odpowiedzi pokazuje, że rzeczywistych nowych elementów do wykonania Azure AD identyfikator użytkownika w polu właściwości właściciela.

    ![Identyfikator właściciela w odpowiedzi JSON](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>Tworzenia projektów od podstaw

Dwa projekty interfejs API sieci Web zostały utworzone przy użyciu szablonu projektu **Azure interfejsu API aplikacji** , a Zastępowanie kontrolera wartości domyślne kontrolera listy zadań. 

Aby dowiedzieć się, jak utworzyć aplikację jednostronicowy AngularJS wewnętrznej 2 interfejsu API sieci Web, zobacz [ręce na Ćwiczenia: tworzenie jednej strony aplikacji (SPA) przy użyciu interfejsu API sieci Web programu ASP.NET i Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Aby dowiedzieć się, jak dodawać kod uwierzytelniania Azure AD zobacz następujące zasoby:

* [Zabezpieczanie AngularJS pojedynczej strony aplikacji z usługą Azure Active Directory](../active-directory/active-directory-devquickstarts-angular.md).
* [Wprowadzenie do wersji ADAL JS 1](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Rozwiązywanie problemów

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Upewnij się, że użytkownik nie należy mylić ToDoListAPI (warstwa środkowa) i ToDoListDataAPI (Warstwa danych). Na przykład Sprawdź, czy dodana uwierzytelniania do aplikacji, środkowa warstwa interfejsu API, nie warstwie danych. 
* Upewnij się, że kod źródłowy AngularJS odwołuje się adres URL aplikacji warstwa środkowa interfejsu API (ToDoListAPI, nie ToDoListDataAPI) i poprawne Azure AD identyfikatora klienta. 

## <a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak za pomocą aplikacji usługi uwierzytelniania dla aplikacji interfejsu API i jak połączenia aplikacji interfejsu API przy użyciu bibliotece ADAL JS. W następnej samouczku dowiesz się, jak [bezpiecznego dostępu do aplikacji interfejsu API do usługi scenariuszach](app-service-api-dotnet-service-principal-auth.md).

