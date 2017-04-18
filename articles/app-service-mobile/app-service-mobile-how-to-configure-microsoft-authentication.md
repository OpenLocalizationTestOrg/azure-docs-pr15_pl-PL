<properties
    pageTitle="Jak skonfigurować Account Microsoft uwierzytelniania dla aplikacji usług aplikacji"
    description="Dowiedz się, jak skonfigurować Account Microsoft uwierzytelniania dla aplikacji usługi aplikacji."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Jak skonfigurować aplikację usługi aplikacji do logowania się do programu Microsoft Account za pomocą

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

W tym temacie pokazano, jak skonfigurować usługę Azure aplikacji do używania Account firmy Microsoft jako dostawcę uwierzytelniania. 

## <a name="register-microsoft-account"> </a>Zarejestrować aplikację za pomocą konta Microsoft

1. Zaloguj się do [portalu Azure]i przejdź do aplikacji. Kopiowanie adresu **URL**, później służy do konfigurowania aplikacji za pomocą Account Microsoft.

2. Przejdź do strony [Moje aplikacje] w programie Microsoft Account Developer Center i zaloguj się przy użyciu konta Microsoft, jeśli jest to wymagane.

3. Kliknij pozycję **Dodaj aplikację**, a następnie wpisz nazwę aplikacji, a następnie kliknij pozycję **Utwórz aplikację**.

4. Zanotuj **Identyfikator aplikacji**, jak będą potrzebne później. 

5. W obszarze "Platform," kliknij pozycję **Dodaj platformy** i wybierz pozycję "Web".

6. W obszarze "Przekierowywanie identyfikatory URI" podać punkt końcowy dla aplikacji, a następnie kliknij przycisk **Zapisz**. 
 
    >[AZURE.NOTE]Usługi przekierowania URI jest adres URL aplikacji dołączony ścieżkę _/.auth/login/microsoftaccount/callback_. Na przykład `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Upewnij się, że korzystasz z systemu HTTPS.

7. W obszarze "Hasła aplikacji", kliknij pozycję **Generuj nowe hasło**. Zanotuj wartość, która jest wyświetlana. Gdy zechcesz opuścić stronę go nie pojawi się ponownie.


    > [AZURE.IMPORTANT] Hasło jest ważne poświadczenie. Nie udostępnić wszystkim osobom, hasło lub jej rozpowszechnianie aplikacji klienckiej.

## <a name="secrets"> </a>Dodaj konto Microsoft informacje w aplikacji aplikacji usługi

1. Ponownie w [Azure portal], przejdź do aplikacji, kliknij przycisk **Ustawienia** > **uwierzytelniania i autoryzacji**.

2. Jeśli uwierzytelnianie / autoryzacji funkcja nie jest włączona, **Włącz ją**.

3. Kliknij pozycję **Konta Microsoft**. Wklej wartości Identyfikator aplikacji i hasło, które poprzednio uzyskano i opcjonalnie włączyć dowolnego zakresy, które aplikacja wymaga. Kliknij przycisk **OK**.

    ![][1]

    Domyślnie aplikacji usługi zapewnia uwierzytelnianie, ale nie ogranicza autoryzowanych dostępu do zawartości witryny i interfejsów API. Użytkownicy muszą zezwolić w kodzie aplikacji.

4. (Opcjonalnie) Aby ograniczyć dostęp do witryny, aby tylko użytkowników uwierzytelnionych przez konta Microsoft, należy ustawić **akcję do wykonania po żądanie nie jest uwierzytelniony** do **Konta Microsoft**. Wymaga to, że wszystkie żądania można uwierzytelnić, a wszystkie żądania nieuwierzytelnionych nastąpi przekierowanie do konta Microsoft w celu uwierzytelniania.

5. Kliknij przycisk **Zapisz**.

Teraz możesz przystąpić do korzystania z programu Microsoft Account uwierzytelniania w aplikacji.

## <a name="related-content"> </a>Dotyczące zawartości

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Aplikacje]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portal]: https://portal.azure.com/
