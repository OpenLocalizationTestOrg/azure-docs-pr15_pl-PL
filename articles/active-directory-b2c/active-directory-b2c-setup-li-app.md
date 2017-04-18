<properties
    pageTitle="Azure Active Directory B2C: Konfiguracja usługi LinkedIn | Microsoft Azure"
    description="Zapewnienie konsumentów z kontami usługi LinkedIn w aplikacjach, które są zabezpieczone przez Azure Active Directory B2C zapisów i logowanie"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Podaj zapisów i logowania dla konsumentów z kontami usługi LinkedIn

## <a name="create-a-linkedin-application"></a>Tworzenie aplikacji LinkedIn

Aby użyć usługi LinkedIn jako dostawcy tożsamości w B2C usługi Azure Active Directory (Azure AD), należy utworzyć aplikację LinkedIn i dostarczyć prawo parametry. Potrzebne konto usługi LinkedIn, aby to zrobić. Jeśli nie istnieje, możesz go uzyskać u [https://www.linkedin.com/](https://www.linkedin.com/).

1. Przejdź do [witryny sieci Web deweloperów usługi LinkedIn](https://www.developer.linkedin.com/) i zaloguj się przy użyciu poświadczeń konta usługi LinkedIn.
2. Kliknij pozycję **Moje aplikacje** w na górnym pasku menu, a następnie kliknij pozycję **Utwórz aplikację**.

    ![LinkedIn — nowej aplikacji](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. Wypełnij informacje (**Nazwa firmy**, **Nazwa**, **Opis**, **Adres URL aplikacji Logo**, **Stosowania**, **Adres URL witryny sieci Web**, **Firmową pocztę E-mail** i **Numer telefonu służbowego**) w formularzu **Utwórz nową aplikację** .
4. Zgadza się **LinkedIn interfejsu API warunki użytkowania** i kliknij przycisk **Prześlij**.

    ![LinkedIn — aplikacja rejestru](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. Skopiuj wartości **Identyfikator klienta** i **Hasła klienta**. (Można je znaleźć w obszarze **Kluczy uwierzytelniania**). Konieczne będzie, oba z nich, aby skonfigurować LinkedIn jako dostawcy tożsamości w dzierżawie.

    >[AZURE.NOTE] **Tajny klienta** jest ważne poświadczenie.

6. Wprowadź `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` w polu **Dozwolone przekierowywanie adresów URL** (w obszarze **OAuth 2.0**). **{Dzierżawy}** należy zastąpić nazwę Twojej dzierżawy (na przykład contoso.onmicrosoft.com). Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **Aktualizuj**. Wartość **{dzierżawy}** jest uwzględniana wielkość liter.

    ![LinkedIn - konfiguracji aplikacji](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Konfigurowanie usługi LinkedIn jako dostawcy tożsamości w dzierżawie

1. Wykonaj poniższe czynności Przejdź [do karta funkcje B2C](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) portalu Azure.
2. Karta funkcje B2C polecenie od **dostawcy tożsamości**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. Podaj przyjazną **nazwę** konfiguracji dostawcy tożsamości. Na przykład wpisz "LI".
5. Kliknij **Typ dostawcy tożsamości**, wybierz pozycję **LinkedIn**i kliknij **przycisk OK**.
6. Kliknij pozycję **Konfiguruj tego dostawcy tożsamości** i wprowadź identyfikator klienta i hasło klienta aplikacji LinkedIn, który został utworzony wcześniej.
7. Kliknij **przycisk OK** , a następnie kliknij przycisk **Utwórz** , aby zapisać konfigurację usługi LinkedIn.
