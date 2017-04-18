<properties
    pageTitle="Jak skonfigurować uwierzytelnianie Facebook dla aplikacji usług aplikacji"
    description="Dowiedz się, jak skonfigurować uwierzytelnianie Facebook dla aplikacji usługi aplikacji."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Jak skonfigurować logowania Facebook za pomocą aplikacji aplikacji usługi

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

W tym temacie pokazano, jak skonfigurować usługę Azure aplikacji do korzystania z serwisu Facebook jako dostawcy uwierzytelniania.

Aby wykonać procedurę opisaną w tym temacie, musisz mieć konto Facebook, zawierający adres e-mail zatwierdzonych oraz numer telefonu komórkowego. Aby utworzyć nowe konto w serwisie Facebook, przejdź do [facebook.com].

## <a name="register"> </a>Zarejestrować aplikację z serwisem Facebook

1. Zaloguj się do [portalu Azure]i przejdź do aplikacji. Kopiowanie adresu **URL**. Użyjemy to do skonfigurowania aplikacji Facebook.

2. W innym oknie przeglądarki przejdź do witryny [Deweloperów Facebook] i zaloguj się przy użyciu swoje poświadczenia konta w serwisie Facebook.

3. (Opcjonalnie) Jeśli nie masz już zarejestrowana, kliknij pozycję **aplikacje** > **zarejestrować jako deweloper**, a następnie zaakceptować zasady i postępuj zgodnie z instrukcjami rejestracji.

4. Kliknij pozycję **Moje aplikacje** > **Dodawanie nowej aplikacji** > **witryny sieci Web** > **Pomiń i Utwórz identyfikator aplikacji**. 

5. W polu **Nazwa wyświetlana**wpisz unikatową nazwę aplikacji, wpisz adres **E-mail kontaktu**, wybierz **kategorię** dla aplikacji sieci, a następnie kliknij pozycję **Utwórz identyfikator aplikacji** i ukończyć sprawdzania zabezpieczeń. Zostanie wyświetlone pulpit nawigacyjny programisty dla nowej aplikacji Facebook.

6. W obszarze "Facebook logowania", kliknij pozycję **Wprowadzenie**. Dodawanie aplikacji **Przekierowywać URI** **prawidłowych OAuth**przekierowywać identyfikatory URI, a następnie kliknij przycisk **Zapisz zmiany**. 

    > [AZURE.NOTE] Usługi przekierowania URI jest adres URL aplikacji dołączony ścieżkę _/.auth/login/facebook/callback_. Na przykład `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Upewnij się, że korzystasz z systemu HTTPS.

6. W obszarze nawigacji po lewej stronie kliknij pozycję **Ustawienia**. W polu **Hasło aplikacji** kliknij przycisk **Pokaż**, podaj swoje hasło, jeśli wymagane, a następnie zanotuj wartości **Identyfikator aplikacji** i **Hasła aplikacji**. Można użyć tych później do konfigurowania aplikacji platformy Azure.

    > [AZURE.IMPORTANT] Hasło aplikacji jest ważne poświadczenie. Nie udostępnić wszystkim osobom tego hasła ani jej rozpowszechnianie aplikacji klienckiej.

7. Kontem w serwisie Facebook, którego użyto do rejestrowania aplikacji jest administratorem aplikacji. W tym momencie tylko administratorzy mogą zalogować się do tej aplikacji. Aby uwierzytelnić innych kont w serwisie Facebook, kliknij kartę **Recenzja aplikacji** i Włącz **< nazwa użytkownika aplikacji > Udostępnij publicznie** włączyć ogólne dostępie przy użyciu funkcji uwierzytelniania Facebook.

## <a name="secrets"> </a>Facebook dodać informacje do aplikacji

1. W programie [Azure portal]przejdź do aplikacji. Kliknij pozycję **Ustawienia** > **uwierzytelniania i autoryzacji**i upewnij się, że **Uwierzytelniania usługi aplikacji** znajduje się **na**.

2. Kliknij pozycję **Facebook**, Wklej wartości Identyfikator aplikacji i hasło aplikacji, które poprzednio uzyskano, opcjonalnie można włączyć wszystkie zakresy wymagane przez aplikację, a następnie kliknij **przycisk OK**.

    ![][0]

    Domyślnie aplikacji usługi zapewnia uwierzytelnianie, ale nie ogranicza autoryzowanych dostępu do zawartości witryny i interfejsów API. Użytkownicy muszą zezwolić w kodzie aplikacji.

3. (Opcjonalnie) Aby ograniczyć dostęp do witryny, aby uzyskiwać dostęp tylko użytkownicy uwierzytelnieni w serwisie Facebook, należy ustawić **akcję do wykonania po żądanie nie jest uwierzytelniony** serwisem **Facebook**. Wymaga to, że wszystkie żądania można uwierzytelnić, a wszystkie żądania nieuwierzytelnionych nastąpi przekierowanie do serwisu Facebook dla uwierzytelniania.

4. Po zakończeniu konfigurowania uwierzytelniania, kliknij przycisk **Zapisz**.

Teraz możesz przystąpić do serwisu Facebook za pomocą uwierzytelniania w aplikacji.

## <a name="related-content"> </a>Dotyczące zawartości

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Deweloperów serwisu Facebook]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portal]: https://portal.azure.com/
