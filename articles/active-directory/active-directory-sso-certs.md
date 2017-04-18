<properties
    pageTitle="Jak zarządzać federacyjnych certyfikatów w Azure AD | Microsoft Azure"
    description="Dowiedz się, jak dostosować datę wygaśnięcia własnych certyfikatów Federacji i sposobie ich odnawiania certyfikatów, które niedługo wygaśnie."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Zarządzanie certyfikaty dla federacyjnych logowania jednokrotnego w usłudze Azure Active Directory

W tym artykule opisano, jak często zadawane pytania dotyczące certyfikatów, które usługi Azure Active Directory tworzy się w celu ustalenia federacyjnych logowania jednokrotnego (SSO) aplikacji władz akredytacji bezpieczeństwa.

Ten artykuł dotyczy tylko aplikacje, które są skonfigurowane do używania **Azure AD rejestracji jednokrotnej**, jak pokazano w poniższym przykładzie:

![Azure AD rejestracji jednokrotnej](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>Jak dostosować daty wygaśnięcia dla certyfikatu Federacji

Certyfikaty są domyślnie wygaśnie po dwóch latach. Możesz wybrać datę wygaśnięcia różnych certyfikatu, wykonując poniższe czynności. Uwzględniane zrzutów ekranu za pomocą usług Salesforce przykładach, ale poniższe czynności, można zastosować do dowolnej aplikacji federacyjnych władz akredytacji bezpieczeństwa.

1. W usługi Azure Active Directory na stronie Szybki Start dla aplikacji, kliknij **Konfigurowanie logowania jednokrotnego**.

    ![Otwieranie Kreatora konfiguracji rejestracji Jednokrotnej.](./media/active-directory-sso-certs/config-sso.png)

2. Zaznacz **Azure AD rejestracji jednokrotnej**, a następnie kliknij przycisk **Dalej**.

3. Wpisz w adresie **URL logowania w** aplikacji, a następnie zaznacz pole wyboru dla **Konfigurowanie certyfikatu na potrzeby federacyjnych rejestracji jednokrotnej**. Następnie kliknij przycisk **Dalej**.

    ![Azure AD rejestracji jednokrotnej](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. Na następnej stronie wybierz pozycję **Generuj nowego certyfikatu**, a następnie wybierz, jak długo chcesz certyfikatu, aby był prawidłowy dla. Następnie kliknij przycisk **Dalej**.

    ![Generowanie nowego certyfikatu.](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Następnie wybierz polecenie **Pobierz certyfikat**. Aby dowiedzieć się, jak przekazać certyfikatu do określonej aplikacji władz akredytacji bezpieczeństwa, kliknij pozycję **Widok instrukcje dotyczące konfiguracji**.

    ![Pobierz, a następnie przekaż certyfikatu](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. Certyfikat nie zostanie włączona, aż zaznacz pole wyboru potwierdzenia u dołu okna dialogowego, a następnie naciśnij przycisk Prześlij.

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>Jak odnowić certyfikat, który wkrótce wygaśnie

Czynności odnawiania, jak pokazano poniżej najlepiej powinno spowodować brak przestojów znaczącej dla użytkowników. Zrzuty ekranu, używane w tej sekcji funkcji usług Salesforce jako przykład, ale poniższe czynności, można zastosować do dowolnej aplikacji federacyjnych władz akredytacji bezpieczeństwa.

1. W usługi Azure Active Directory na stronie Szybki Start dla aplikacji, kliknij **Konfigurowanie logowania jednokrotnego**.

    ![Otwieranie Kreatora konfiguracji logowania jednokrotnego](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. Na pierwszej stronie okna dialogowego, **Azure AD rejestracji jednokrotnej** powinna już być zaznaczona, więc kliknij przycisk **Dalej**.

3. Na drugiej stronie zaznacz pole wyboru dla **Konfigurowanie certyfikatu użytego dla federacyjnych rejestracji jednokrotnej**. Następnie kliknij przycisk **Dalej**.

    ![Azure AD rejestracji jednokrotnej](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. Na następnej stronie wybierz pozycję **Generuj nowego certyfikatu**, a następnie wybierz, jak długo chcesz nowego certyfikatu, aby był prawidłowy dla. Następnie kliknij przycisk **Dalej**.

    ![Generowanie nowego certyfikatu.](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Polecenie **Pobierz certyfikat**. Aby pomyślnie rewnew certyfikatu, należy wykonać dwie następujące czynności:

    - Przekaż nowy certyfikat do ekranu konfiguracji rejestracji jednokrotnej aplikacji władz akredytacji bezpieczeństwa. Aby dowiedzieć się, jak to zrobić dla określonej aplikacji władz akredytacji bezpieczeństwa, kliknij pozycję **Widok instrukcje dotyczące konfiguracji**.

    - W Azure AD zaznacz pole wyboru potwierdzenia u dołu okna dialogowego, aby włączyć nowego certyfikatu, a następnie kliknij **Dalej** do przesyłania.

    > [AZURE.IMPORTANT] Rejestracji jednokrotnej do aplikacji zostanie wyłączona moment jedną tych dwóch kroków zostanie zakończone, ale go będą dostępne ponownie po zakończeniu drugim krokiem. W związku z tym aby zminimalizować przestoje, Przygotuj do realizacji obu kroków w krótkim czasu od siebie.

    ![Pobierz, a następnie przekaż certyfikatu](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Dostęp do aplikacji i rejestracji jednokrotnej z usługą Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Rozwiązywanie problemów z opartego na protokole SAML logowania jednokrotnego](active-directory-saml-debugging.md)
