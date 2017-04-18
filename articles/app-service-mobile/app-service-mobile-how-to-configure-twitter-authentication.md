<properties
    pageTitle="Jak skonfigurować Twitter uwierzytelniania dla aplikacji usług aplikacji"
    description="Dowiedz się, jak skonfigurować Twitter uwierzytelniania dla aplikacji usługi aplikacji."
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

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Jak skonfigurować aplikację aplikacji usługi, aby użyć logowania Twitter

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

W tym temacie pokazano, jak skonfigurować usługę aplikacji Azure, aby użyć Twitter jako dostawca uwierzytelniania.

Aby wykonać procedurę opisaną w tym temacie, musisz mieć konto Twitter, zawierającą zatwierdzonych e-mail adres i numer telefonu. Aby utworzyć nowe konto w serwisie Twitter, przejdź do <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Zarejestrować aplikację Twitter


1. Zaloguj się do [portalu Azure]i przejdź do aplikacji. Kopiowanie adresu **URL**. Użyjemy to do skonfigurowania aplikacji Twitter.

2. Przejdź do witryny [Deweloperów serwisu Twitter] , zaloguj się przy użyciu poświadczeń konta Twitter i kliknij pozycję **Utwórz nową aplikację**.

3. Wpisz **nazwę** i **Opis** nowej aplikacji. Wklej **adres URL** aplikacji wartość **witryny sieci Web** . Następnie dla **Adresu zwrotnego URL**Wklej **Zwrotnego adresu URL** skopiowany wcześniej. To jest Centrum aplikacji Mobile dołączony ścieżkę _/.auth/login/twitter/callback_. Na przykład `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Upewnij się, że korzystasz z systemu HTTPS.

3.  U dołu strony przeczytaj i zaakceptuj warunki. Następnie kliknij przycisk **Utwórz aplikacji Twitter**. To rejestruje Wyświetla aplikacji szczegóły aplikacji.

4. Kliknij kartę **Ustawienia** , sprawdź **zezwolić tej aplikacji może być używany, zaloguj się przy użyciu Twitter**, a następnie kliknij pozycję **Ustawienia aktualizacji**.

5. Wybierz kartę **klucze i tokeny dostępu** . Zanotuj wartości **Dla klientów indywidualnych klawisz (API)** i **hasło dla klientów indywidualnych (tajny interfejsu API)**.

    > [AZURE.NOTE] Hasło dla klientów indywidualnych jest ważne poświadczenie. Nie udostępnić wszystkim osobom tego hasła lub rozpowszechnianie go przy użyciu aplikacji.


## <a name="secrets"> </a>Dodawanie serwisu Twitter informacje dla aplikacji

13. W programie [Azure portal]przejdź do aplikacji. Kliknij pozycję **Ustawienia**, a następnie **uwierzytelniania i autoryzacji**.

14. Jeśli uwierzytelnianie / autoryzacji funkcja nie jest włączona, włączanie pracy z wersją programu **na**.

15. Kliknij pozycję **serwisu Twitter**. Wklej wartości, które poprzednio uzyskano identyfikator aplikacji i hasło aplikacji. Kliknij przycisk **OK**.

    ![][1]

    Domyślnie aplikacji usługi zapewnia uwierzytelnianie, ale nie ogranicza autoryzowanych dostępu do zawartości witryny i interfejsów API. Użytkownicy muszą zezwolić w kodzie aplikacji.

17. (Opcjonalnie) Aby ograniczyć dostęp do witryny, aby tylko użytkowników uwierzytelnionych przez Twitter, należy ustawić **akcję do wykonania po żądanie nie jest uwierzytelniony** do **serwisu Twitter**. Wymaga to, że wszystkie żądania można uwierzytelnić, a wszystkie żądania nieuwierzytelnionych nastąpi przekierowanie do Twitter uwierzytelniania.

17. Kliknij przycisk **Zapisz**.

Teraz możesz przystąpić do użycia Twitter dla uwierzytelniania w aplikacji.

## <a name="related-content"> </a>Dotyczące zawartości

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Deweloperów Twitter]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
