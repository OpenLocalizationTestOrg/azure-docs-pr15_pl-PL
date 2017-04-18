<properties
    pageTitle="Jak skonfigurować Google uwierzytelniania dla aplikacji usług aplikacji"
    description="Dowiedz się, jak skonfigurować Google uwierzytelniania dla aplikacji usługi aplikacji."
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

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Jak skonfigurować aplikację aplikacji usługi, aby użyć logowania usługi Google

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

W tym temacie pokazano, jak skonfigurować usługę aplikacji Azure, aby użyć Google jako dostawca uwierzytelniania.

Aby wykonać procedurę opisaną w tym temacie, musi mieć konto Google, które ma adres e-mail zatwierdzonych. Aby utworzyć nowe konto Google, przejdź do [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Zarejestrować aplikację Google

1. Zaloguj się do [portalu Azure]i przejdź do aplikacji. Kopiowanie adresu **URL**, za pomocą której później Konfigurowanie aplikacji usługi Google.

2. Przejdź do witryny sieci Web [interfejsów API usługi Google](http://go.microsoft.com/fwlink/p/?LinkId=268303) , zaloguj się przy użyciu poświadczeń konta Google, kliknij pozycję **Utwórz projekt**, podaj **nazwę projektu**, a następnie kliknij przycisk **Utwórz**.

3. W obszarze **Społecznościowych interfejsy API** kliknij **Google + interfejsu API** , a następnie **Włącz**.

4. W lewym okienku nawigacji, **poświadczenia** > **OAuth zgodę ekranu**, następnie wybierz swój **Adres E-mail**, wprowadź **Nazwy produktu**i kliknij przycisk **Zapisz**.

5. Na karcie **poświadczeń** , kliknij przycisk **Utwórz poświadczenia** > **OAuth identyfikator klienta**, a następnie wybierz pozycję **aplikacja sieci Web**.

6. Wklej skopiowany wcześniej do **Autoryzowanych miejsc pochodzenia JavaScript**aplikacji usługi **adres URL** , a następnie wklej usługi przekierowania URI do **Autoryzowanych przekierowywać identyfikatora URI**. Przekieruj identyfikator URI jest adres URL aplikacji dołączony ścieżkę _/.auth/login/google/callback_. Na przykład `https://contoso.azurewebsites.net/.auth/login/google/callback`. Upewnij się, że korzystasz z systemu HTTPS. Następnie kliknij przycisk **Utwórz**.

7. Na następnym ekranie zanotuj wartości Identyfikator klienta i hasła klienta.


    > [AZURE.IMPORTANT]
    Tajny klienta jest ważne poświadczenie. Nie udostępnić wszystkim osobom tego hasła ani jej rozpowszechnianie aplikacji klienckiej.


## <a name="secrets"> </a>Google dodać informacje do aplikacji

8. W programie [Azure portal]przejdź do aplikacji. Kliknij pozycję **Ustawienia**, a następnie **uwierzytelniania i autoryzacji**.

9. Jeśli uwierzytelnianie / autoryzacji funkcja nie jest włączona, włączanie pracy z wersją programu **na**.

10. Kliknij pozycję **Google**. Wklej wartości Identyfikator aplikacji i hasło aplikacji, które poprzednio uzyskano i opcjonalnie włączyć dowolnego zakresy, które aplikacja wymaga. Kliknij przycisk **OK**.

    ![][1]

    Domyślnie aplikacji usługi zapewnia uwierzytelnianie, ale nie ogranicza autoryzowanych dostępu do zawartości witryny i interfejsów API. Użytkownicy muszą zezwolić w kodzie aplikacji.

17. (Opcjonalnie) Aby ograniczyć dostęp do witryny, aby tylko użytkowników uwierzytelnionych przez Google, ustaw **akcję do wykonania po żądanie nie jest uwierzytelniony** **Google**. Wymaga to, że wszystkie żądania można uwierzytelnić, a wszystkie żądania nieuwierzytelnionych nastąpi przekierowanie do usługi Google uwierzytelniania.

12. Kliknij przycisk **Zapisz**.

Teraz możesz przystąpić do uwierzytelniania w aplikacji za pomocą usługi Google.

## <a name="related-content"> </a>Dotyczące zawartości

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portal]: https://portal.azure.com/

