<properties
    pageTitle="Jak skonfigurować uwierzytelnianie usługi Azure Active Directory dla aplikacji usług aplikacji"
    description="Dowiedz się, jak skonfigurować uwierzytelnianie usługi Azure Active Directory dla aplikacji usługi aplikacji."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
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

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Jak skonfigurować aplikację aplikacji usługi, aby użyć logowania usługi Azure Active Directory

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

W tym temacie przedstawiono sposób konfigurowania usługi aplikacji Azure, aby użyć usługi Azure Active Directory jako dostawca uwierzytelniania.

## <a name="express"> </a>Konfigurowanie usługi Azure Active Directory przy użyciu ustawień express

13. W [portalu Azure]przejdź do aplikacji. Kliknij pozycję **Ustawienia**, a następnie **Uwierzytelniania i autoryzacji**.

14. Jeśli uwierzytelnianie / autoryzacji funkcja nie jest włączona, włączanie pracy z wersją programu **na**.

15. Kliknij pozycję **Usługi Azure Active Directory**, a następnie kliknij przycisk **Express** w **Trybie zarządzania**.

16. Kliknij **przycisk OK** , aby zarejestrować tę aplikację w usłudze Azure Active Directory. Spowoduje to utworzenie nowej rejestracji. Jeśli chcesz zamiast tego wybrać istniejący rejestracji, kliknij przycisk **Wybierz istniejącej aplikacji** , a następnie wyszukaj nazwę utworzonego wcześniej rejestracji na dzierżawy usługi.
Kliknij przycisk Rejestracja, aby go zaznaczyć, a następnie kliknij **przycisk OK**. Kliknij przycisk **OK** na karta Ustawienia usługi Azure Active Directory.

    ![][0]

    Domyślnie aplikacji usługi zapewnia uwierzytelnianie, ale nie ogranicza autoryzowanych dostępu do zawartości witryny i interfejsów API. Użytkownicy muszą zezwolić w kodzie aplikacji.

17. (Opcjonalnie) Aby ograniczyć dostęp do witryny, aby tylko użytkowników uwierzytelnionych przez usługi Azure Active Directory, należy ustawić **akcję do wykonania po żądanie nie jest uwierzytelniony** umożliwiające **zalogowanie się przy użyciu usługi Azure Active Directory**. Wymaga to, że wszystkie żądania można uwierzytelnić, a wszystkie żądania nieuwierzytelnionych nastąpi przekierowanie do usługi Azure Active Directory dla uwierzytelniania.

17. Kliknij przycisk **Zapisz**.

Teraz możesz przystąpić do używania usługi Azure Active Directory dla uwierzytelniania w aplikacji.

## <a name="advanced"> </a>(Metoda alternatywny) ręczne konfigurowanie usługi Azure Active Directory przy użyciu ustawień zaawansowanych
Możesz również wybrać przekazywać ustawienia konfiguracji ręcznie. Jest to preferowany rozwiązanie, jeśli dzierżawy AAD, które mają być używane różni się od dzierżawy, z którym możesz zalogować się do Azure. Aby zakończyć konfigurację, musisz najpierw utworzyć rejestracji w usługi Azure Active Directory, a następnie należy podać niektóre szczegóły rejestracji do aplikacji usługi.

### <a name="register"> </a>Zarejestrować aplikację usługi Azure Active Directory

1. Zaloguj się do [portalu Azure]i przejdź do aplikacji. Kopiowanie adresu **URL**. Użyjemy to do skonfigurowania aplikacji usługi Azure Active Directory.

3. Zaloguj się do [portalu klasyczny Azure] i przejdź do **Usługi Active Directory**.

    ![][2]

4. Zaznacz katalogu, a następnie wybierz kartę **aplikacji** u góry. U dołu w celu utworzenia nowej rejestracji aplikacji, kliknij przycisk **Dodaj** .

5. Kliknij pozycję **Dodaj aplikację, którą rozwija się w mojej organizacji**.

6. W kreatorze Dodawanie aplikacji wpisz **nazwę** aplikacji, a następnie kliknij typ **Interfejs API sieci Web i/lub aplikacji sieci Web** . Następnie kliknij, aby kontynuować.

7. W polu **Adres URL logowania na** Wklej skopiowany wcześniej adres URL aplikacji. Wprowadź tego samego adresu URL w polu **Aplikacji identyfikator URI** . Następnie kliknij, aby kontynuować.

8. Po dodaniu aplikacji kliknij kartę **Konfiguruj** . Edytowanie **Adresu URL odpowiedź** w obszarze **logowania jednokrotnego** jako adres URL aplikacji dołączony ścieżkę _/.auth/login/aad/callback_. Na przykład `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Upewnij się, że korzystasz z systemu HTTPS.

    ![][3]

9. Kliknij przycisk **Zapisz**. Następnie skopiuj **Identyfikator klienta** aplikacji. Skonfiguruj aplikację, aby to późniejszego użycia.

10. Na dolnym pasku poleceń kliknij **Widok punkty końcowe**i następnie skopiuj adres URL **Dokumentu metadanych Federacji** i Pobierz ten dokument lub przejdź do niego w przeglądarce.

11. W elemencie głównym **EntityDescriptor** powinny być atrybut **identyfikator jednostki** formularza `https://sts.windows.net/` następuje GUID określonych do Twojej dzierżawy (nazywane "identyfikator dzierżawy"). Skopiuj tę wartość — będzie służyć jako adres **URL wystawcy**. Skonfiguruj aplikację, aby to późniejszego użycia.

### <a name="secrets"> </a>Informacji Dodawanie usługi Azure Active Directory do aplikacji

13. W programie [Azure portal]przejdź do aplikacji. Kliknij pozycję **Ustawienia**, a następnie **Uwierzytelniania i autoryzacji**.

14. Jeśli nie włączono funkcji uwierzytelniania i autoryzacji, wyłączyć pracy z wersją programu **na**.

15. Kliknij **Usługi Azure Active Directory**, a następnie kliknij przycisk **Zaawansowane** w obszarze **Zarządzanie tryb**. Wklej wartość Identyfikator klienta i adres URL wystawcy uzyskaną wcześniej. Kliknij przycisk **OK**.

    ![][1]

    Domyślnie aplikacji usługi zapewnia uwierzytelnianie, ale nie ogranicza autoryzowanych dostępu do zawartości witryny i interfejsów API. Użytkownicy muszą zezwolić w kodzie aplikacji.

17. (Opcjonalnie) Aby ograniczyć dostęp do witryny, aby tylko użytkowników uwierzytelnionych przez usługi Azure Active Directory, należy ustawić **akcję do wykonania po żądanie nie jest uwierzytelniony** umożliwiające **zalogowanie się przy użyciu usługi Azure Active Directory**. Wymaga to, że wszystkie żądania można uwierzytelnić, a wszystkie żądania nieuwierzytelnionych nastąpi przekierowanie do usługi Azure Active Directory dla uwierzytelniania.

17. Kliknij przycisk **Zapisz**.

Teraz możesz przystąpić do używania usługi Azure Active Directory dla uwierzytelniania w aplikacji.

## <a name="optional-configure-a-native-client-application"></a>(Opcjonalnie) Konfigurowanie aplikacji klientami

Azure Active Directory umożliwia również zarejestrować natywnych klientów, która zapewnia większą kontrolę nad uprawnienia mapowanie. Potrzebujesz to jeśli chcesz wykonać logowania przy użyciu biblioteki, takie jak **Active Directory Authentication Library**.

1. Przejdź do **Usługi Active Directory** w [portal Azure klasyczny].

2. Zaznacz katalogu, a następnie wybierz kartę **aplikacji** u góry. U dołu w celu utworzenia nowej rejestracji aplikacji, kliknij przycisk **Dodaj** .

3. Kliknij pozycję **Dodaj aplikację, którą rozwija się w mojej organizacji**.

4. W kreatorze Dodawanie aplikacji wpisz **nazwę** aplikacji, a następnie kliknij typ **Natywnej aplikacji** . Następnie kliknij, aby kontynuować.

5. W oknie dialogowym **Przekierowywać URI** wprowadź witryny _/.auth/login/done_ punktu końcowego, przy użyciu schematu HTTPS. Ta wartość powinna być podobna do _https://contoso.azurewebsites.net/.auth/login/done_. W przypadku tworzenia aplikacji systemu Windows, użyj zamiast tego [pakietu SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) jako identyfikator URI.

6. Po dodaniu natywnej aplikacji kliknij kartę **Konfiguruj** . Znajdowanie **Identyfikator klienta** i zanotuj tę wartość.

7. Przewiń stronę w dół do sekcji **uprawnienia do innych aplikacji** , a następnie kliknij polecenie **Dodaj aplikację**.

8. Wyszukaj wcześniej zarejestrowanego aplikacji sieci web i kliknij ikonę plusa. Następnie kliknij przycisk Sprawdź, aby zamknąć okno dialogowe. Jeśli aplikacja sieci web nie można znaleźć, przejdź do swojej rejestracji i dodać nowy adres URL odpowiedzi (przykład wersja HTTP adresu URL bieżącej), kliknij przycisk Zapisz, a następnie powtórz te kroki — aplikacja powinna wyświetlane na liście.

9. Na nowy wpis, który właśnie dodany Otwórz listę rozwijaną **Delegowane uprawnienia** i wybierz **programu Access (argumentu)**. Następnie kliknij przycisk **Zapisz**.

Masz już skonfigurowane aplikacji klientami, które mogą uzyskiwać dostęp do aplikacji aplikacji usługi.

## <a name="related-content"> </a>Dotyczące zawartości

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Portal Azure klasyczny]: https://manage.windowsazure.com/
[alternative method]:#advanced
