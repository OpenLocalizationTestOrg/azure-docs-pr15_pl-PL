<properties
    pageTitle="Azure Active Directory B2C: Konfiguracja Amazon | Microsoft Azure"
    description="Podaj zapisów i logowania dla konsumentów z kontami Amazon w aplikacjach, które są zabezpieczone przez Azure Active Directory B2C."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Podaj zapisów i logowania dla konsumentów z kontami Amazon

## <a name="create-an-amazon-application"></a>Tworzenie aplikacji Amazon

Aby użyć Amazon jako dostawcy tożsamości w B2C usługi Azure Active Directory (Azure AD), należy utworzyć aplikację Amazon i dostarczyć prawo parametry. Wymagane jest konto Amazon, aby to zrobić. Jeśli nie istnieje, możesz go uzyskać u [http://www.amazon.com/](http://www.amazon.com/).

1. Przejdź do [Centrum deweloperów Amazon](https://login.amazon.com/) i zaloguj się przy użyciu poświadczeń konta Amazon.
2. Jeśli jeszcze tego nie zrobiono, kliknij pozycję **Utwórz konto**, postępuj zgodnie z instrukcjami rejestracji Deweloper i zaakceptować zasady.
3. Kliknij przycisk **Zarejestruj nowej aplikacji**.

    ![Rejestrowanie nowej aplikacji w witrynie sieci Web Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. Udostępnia informacje o aplikacji (**Nazwa**, **Opis**i **Adres URL powiadomienie o prywatności**) i kliknij przycisk **Zapisz**.

    ![Przekazywanie informacji o aplikacji do rejestrowania nowej aplikacji u Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. W sekcji **Ustawienia sieci Web** skopiuj wartości **Identyfikator klienta** i **Hasła klienta**. (Musisz kliknij przycisk **Pokaż hasło** , aby zobaczyć.) Należy dysponować ich tak, aby skonfigurować Amazon jako dostawcy tożsamości w dzierżawie. Kliknij przycisk **Edytuj** w dolnej sekcji. **Tajny klienta** jest ważne poświadczenie.

    ![Zapewnienie identyfikator klienta i tajny klienta dla nowej aplikacji u Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. Wprowadź `https://login.microsoftonline.com` w polu **Dozwolone miejsc pochodzenia JavaScript** i `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` w polu **Dozwolone adresy URL zwracana** . **{Dzierżawy}** należy zastąpić nazwę Twojej dzierżawy (na przykład contoso.onmicrosoft.com). Kliknij przycisk **Zapisz**. Wartość **{dzierżawy}** jest uwzględniana wielkość liter.

    ![Zapewnienie miejsc pochodzenia JavaScript i zwrócić adresy URL dla nowej aplikacji u Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Konfigurowanie Amazon jako dostawcy tożsamości w dzierżawie

1. Wykonaj poniższe czynności Przejdź [do karta funkcje B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portalu Azure.
2. Karta funkcje B2C polecenie od **dostawcy tożsamości**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. Podaj przyjazną **nazwę** konfiguracji dostawcy tożsamości. Na przykład "Amzn".
5. Kliknij **Typ dostawcy tożsamości**, wybierz pozycję **Amazon**, a następnie kliknij **przycisk OK**.
6. Kliknij pozycję **Konfiguruj tego dostawcy tożsamości** i wprowadź identyfikator klienta i hasło klienta stosowania Amazon, który został utworzony wcześniej.
7. Kliknij **przycisk OK** , a następnie kliknij przycisk **Utwórz** , aby zapisać konfigurację Amazon.
