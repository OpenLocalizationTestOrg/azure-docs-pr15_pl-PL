<properties
    pageTitle="Azure Active Directory B2C: Google + konfiguracji | Microsoft Azure"
    description="Podaj zapisów i logowania dla konsumentów z kontami usługi Google + w aplikacjach, które są zabezpieczone przez Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Podaj zapisów i logowania dla konsumentów z kontami usługi Google +

## <a name="create-a-google-application"></a>Tworzenie aplikacji Google +

Aby użyć Google + jako dostawcy tożsamości w B2C usługi Azure Active Directory (Azure AD), należy utworzyć aplikację Google + i dostarczyć prawo parametry. Wymagane jest konto Google + to zrobić. Jeśli nie istnieje, możesz go uzyskać u [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Przejdź do [Konsoli deweloperów Google](https://console.developers.google.com/) i zaloguj się przy użyciu poświadczeń konta Google +.
2. Kliknij pozycję **Utwórz projekt**, wprowadź **nazwę projektu**, a następnie kliknij przycisk **Utwórz**.

    ![Google + — wprowadzenie](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google + - nowego projektu](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Kliknij pozycję **Menedżer interfejsu API** , a następnie kliknij pozycję **poświadczeń** w lewym okienku nawigacji.
4. Kliknij kartę **OAuth zgodę ekranu** u góry.

    ![Google + - poświadczeń](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Wybierz lub Określ prawidłowy **Adres E-mail**, podaj **nazwę produktu**i kliknij przycisk **Zapisz**.

    ![Google + - OAuth zgody ekranu](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Kliknij przycisk **nowe poświadczenia** , a następnie wybierz **identyfikator klienta OAuth**.

    ![Google + - OAuth zgody ekranu](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. W obszarze **Typ aplikacji**wybierz **aplikację sieci Web**.

    ![Google + - OAuth zgody ekranu](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Podaj **nazwę** aplikacji, wprowadź `https://login.microsoftonline.com` w polu **miejsc pochodzenia autoryzowanych JavaScript** i `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` w polu **autoryzowane przekierowywać identyfikatory URI** . **{Dzierżawy}** należy zastąpić nazwę Twojej dzierżawy (na przykład contosob2c.onmicrosoft.com). Wartość **{dzierżawy}** jest uwzględniana wielkość liter. Kliknij przycisk **Utwórz**.

    ![Google + — tworzenie identyfikator klienta](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Skopiuj wartości **Identyfikator klienta** i **hasła klienta**. Konieczne będzie, oba z nich, aby skonfigurować Google + jako dostawcy tożsamości w dzierżawie. **Tajny klienta** jest ważne poświadczenie.

    ![Google + - tajny klienta](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Konfigurowanie usługi Google + jako dostawcy tożsamości w dzierżawie

1. Wykonaj poniższe czynności Przejdź [do karta funkcje B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portalu Azure.
2. Karta funkcje B2C polecenie od **dostawcy tożsamości**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. Podaj przyjazną **nazwę** konfiguracji dostawcy tożsamości. Na przykład wpisz "G +".
5. Kliknij **Typ dostawcy tożsamości**, wybierz pozycję **Google**i kliknij **przycisk OK**.
6. Kliknij pozycję **Konfiguruj tego dostawcy tożsamości** i wprowadź identyfikator klienta i hasło klienta aplikacji Google +, który został utworzony wcześniej.
7. Kliknij **przycisk OK** , a następnie kliknij przycisk **Utwórz** , aby zapisać konfigurację Google +.
