<properties
    pageTitle="Azure Active Directory B2C: Konfiguracja konta Microsoft | Microsoft Azure"
    description="Podaj zapisów i logowania dla konsumentów przy użyciu konta Microsoft w aplikacjach, które są zabezpieczone przez Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Podaj zapisów i logowania dla konsumentów przy użyciu konta Microsoft

## <a name="create-a-microsoft-account-application"></a>Tworzenie aplikacji konta Microsoft

Aby użyć konta Microsoft jako dostawcy tożsamości w B2C usługi Azure Active Directory (Azure AD), należy utworzyć aplikację konta Microsoft i dostarczyć prawo parametrów. Potrzebujesz konto Microsoft, aby to zrobić. Jeśli nie istnieje, możesz go uzyskać u [https://www.live.com/](https://www.live.com/).

1. Przejdź do [Portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) i zaloguj się przy użyciu poświadczeń konta Microsoft.
2. Kliknij pozycję **Dodaj aplikację**.

    ![Microsoft kont — Dodawanie nowej aplikacji](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. Podaj **nazwę** aplikacji, a następnie kliknij pozycję **Utwórz aplikację**.

    ![Konto Microsoft — Nazwa aplikacji](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. Skopiuj wartość **Identyfikatora aplikacji**. Potrzebujesz, aby skonfigurować konto Microsoft jako dostawcy tożsamości w dzierżawie

    ![Konto Microsoft - identyfikator aplikacji](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. Kliknij na **platformie Dodaj** i wybierz **sieci Web**.

    ![Microsoft kont — Dodawanie platformy](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Konto Microsoft — sieci Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. Wprowadź `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` w polu **Przekieruj identyfikatory URI** . **{Dzierżawy}** należy zastąpić nazwę Twojej dzierżawy (na przykład contosob2c.onmicrosoft.com).

    ![Konto Microsoft — adres URL przekierowania](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. Kliknij pozycję **Generuj nowe hasło** w sekcji **Hasła aplikacji** . Skopiuj nowe hasło, które są wyświetlane na ekranie. Potrzebujesz, aby skonfigurować konto Microsoft jako dostawcy tożsamości w dzierżawie To hasło jest ważne poświadczenie.

    ![Microsoft kont — Generuj nowe hasło](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Konto Microsoft — nowe hasło](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. Zaznacz pole informacją, że **obsługuje zestaw Live SDK** w sekcji **Opcje zaawansowane** . Kliknij przycisk **Zapisz**.

    ![Konto Microsoft — Obsługa Live SDK](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Konfigurowanie konta Microsoft jako dostawcy tożsamości w dzierżawie

1. Wykonaj poniższe czynności Przejdź [do karta funkcje B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portalu Azure.
2. Karta funkcje B2C polecenie od **dostawcy tożsamości**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. Podaj przyjazną **nazwę** konfiguracji dostawcy tożsamości. Na przykład wpisz "MSA".
5. Kliknij **Typ dostawcy tożsamości**, zaznacz **konto Microsoft**, a następnie kliknij **przycisk OK**.
6. Kliknij pozycję **Konfiguruj tego dostawcy tożsamości** i wprowadź identyfikator aplikacji i hasło aplikacji konta Microsoft, utworzony wcześniej.
7. Kliknij **przycisk OK** , a następnie kliknij przycisk **Utwórz** , aby zapisać konfigurację konta Microsoft.
