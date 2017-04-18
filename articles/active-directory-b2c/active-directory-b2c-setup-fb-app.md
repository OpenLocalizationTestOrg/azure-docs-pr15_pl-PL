<properties
    pageTitle="Azure Active Directory B2C: Konfiguracja Facebook | Microsoft Azure"
    description="Podaj zapisów i logowania dla konsumentów z kontami Facebook w aplikacji, które są zabezpieczone przez Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Podaj zapisów i logowania dla konsumentów z kontami Facebook

## <a name="create-a-facebook-application"></a>Tworzenie aplikacji Facebook

Aby użyć Facebook jako dostawcy tożsamości w B2C usługi Azure Active Directory (Azure AD), należy utworzyć aplikację serwisu Facebook i dostarczyć prawo parametrów. Wymagane jest konto Facebook, aby to zrobić. Jeśli nie istnieje, możesz go uzyskać u [https://www.facebook.com/](https://www.facebook.com/).

1. Przejdź do witryny sieci Web [Facebook dla deweloperów](https://developers.facebook.com/) i zaloguj się przy użyciu poświadczeń konta serwisu Facebook.
2. Jeśli jeszcze tego nie zrobiono, musisz zarejestrować jako deweloper Facebook. Aby to zrobić, kliknij przycisk **Zarejestruj** (w prawym górnym rogu strony), Zaakceptuj zasad w serwisie Facebook i wykonaj kroki od rejestracji.
3. Kliknij pozycję **Moje aplikacje** , a następnie kliknij przycisk **Dodaj nową aplikację**. Wybierz **witrynę sieci Web** jako platformę, a następnie kliknij **Pomiń i Utwórz identyfikator aplikacji**.

    ![Facebook — Dodawanie nowej aplikacji](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebook — Dodawanie nowej aplikacji - witryny sieci Web](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebook — tworzenie identyfikator aplikacji](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. W formularzu podaj **Nazwę wyświetlaną**, prawidłowy **Adres E-mail kontaktu**, odpowiednią **kategorię**, a następnie kliknij przycisk **Utwórz identyfikator aplikacji**. Należy zaakceptować zasady platformy Facebook i uzupełnić kontroli bezpieczeństwa.

    ![Facebook — utworzyć nowy identyfikator aplikacji](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. Na lewym pasku nawigacyjnym kliknij pozycję **Ustawienia** .
6. Kliknij przycisk **+ Dodaj Platform** , a następnie wybierz **witrynę sieci Web**.

    ![Facebook — ustawienia](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![— Ustawienia — witryny internetowej](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. Wprowadź [https://login.microsoftonline.com/](https://login.microsoftonline.com/) w polu **Adres URL witryny** , a następnie kliknij przycisk **Zapisz zmiany**.

    ![Facebook — adres URL witryny](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. Skopiuj wartość **Identyfikator aplikacji**. Kliknij przycisk **Pokaż** i skopiuj wartość **Hasło aplikacji**. Konieczne będzie, oba z nich, aby skonfigurować Facebook jako dostawcy tożsamości w dzierżawie. **Hasło aplikacji** jest ważne poświadczenie.

    ![Facebook - identyfikator aplikacji i hasła aplikacji](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. Kliknij przycisk **+ Dodaj produktu** na lewym pasku nawigacyjnym, a następnie przycisk **Wprowadzenie** obok **Logowania Facebook**.

    ![Facebook — logowania Facebook](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. Wprowadź `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` w polu **prawidłowy OAuth przekierowywać identyfikatory URI** w sekcji **Ustawienia OAuth klienta** . **{Dzierżawy}** należy zastąpić nazwę Twojej dzierżawy (na przykład contosob2c.onmicrosoft.com). Kliknij przycisk **Zapisz zmiany** u dołu strony.

    ![Facebook — OAuth przekierowania URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. Aby zwiększyć bezpieczeństwo aplikacji Facebook mogą być używane przez Azure AD B2C, musisz udostępnić publicznie. Można to zrobić, klikając pozycję **Recenzja aplikacji** w lewym okienku nawigacji i włączanie Przełącz u góry strony, aby **Tak** , a następnie klikając przycisk **Potwierdź**.

    ![Facebook — publicznej aplikacji](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Konfigurowanie serwisu Facebook jako dostawcy tożsamości w dzierżawie

1. Wykonaj poniższe czynności Przejdź [do karta funkcje B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portalu Azure.
2. Karta funkcje B2C polecenie od **dostawcy tożsamości**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. Podaj przyjazną **nazwę** konfiguracji dostawcy tożsamości. Na przykład wpisz "FB".
5. Kliknij **Typ dostawcy tożsamości**, wybierz pozycję **Facebook**, a następnie kliknij **przycisk OK**.
6. Kliknij pozycję **Konfiguruj tego dostawcy tożsamości** i wprowadź identyfikator aplikacji oraz aplikacji hasło (aplikacji Facebook, który został utworzony wcześniej) w polu **Identyfikator klienta** i **tajny klienta** odpowiednio pola.
7. Kliknij **przycisk OK**, a następnie kliknij przycisk **Utwórz** , aby zapisać konfigurację Facebook.
