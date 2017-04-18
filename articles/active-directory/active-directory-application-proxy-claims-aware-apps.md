<properties
    pageTitle="Praca z roszczeń pamiętać o aplikacji w serwera Proxy aplikacji"
    description="Opisano, jak rozpocząć pracę z serwera Proxy aplikacji Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>



# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Praca z roszczeń pamiętać o aplikacji w serwera Proxy aplikacji

Aplikacje pamiętać o oświadczeniach wykonać przekierowanie do zabezpieczeń tokenu usługi (STS), która z kolei żąda poświadczeń od użytkownika w zamian za tokenu przed przekierowaniem użytkownika do aplikacji. Aby włączyć serwer Proxy aplikacji do pracy z tych przekierowań, należy podjąć następujące kroki.

## <a name="prerequisites"></a>Wymagania wstępne
Przed wykonaniem tej procedury, upewnij się, że STS aplikacji pamiętać o oświadczeniach przekierowuje do jest dostępna spoza sieci lokalnej.

## <a name="azure-classic-portal-configuration"></a>Azure klasyczny Konfiguracja portalu

1. Publikowanie aplikacji zgodnie z instrukcjami opisanych w [aplikacjach Publikuj z serwera Proxy aplikacji](active-directory-application-proxy-publish.md).
2. Na liście aplikacji wybierz odpowiednią aplikację pamiętać o oświadczeniach, a następnie kliknij przycisk **Konfiguruj**.
3. Jeśli wybierzesz **przekazywanie** jako **Metody było wstępnie uwierzytelnić**upewnij się zaznaczyć **HTTPS** jako schemat **Zewnętrznego adresu URL** .
4. Jeśli wybierzesz **Usługi Azure Active Directory** jako **Metody było wstępnie uwierzytelnić**, wybierz opcję **Brak** jako **Wewnętrzny metody uwierzytelniania**.


## <a name="adfs-configuration"></a>Konfiguracja usług ADFS organizacji

1. Otwórz okno Zarządzanie ADFS.
2. Przejdź do pozycji **Strona Polegaj zaufanie**, kliknij prawym przyciskiem myszy aplikację publikowania z serwera Proxy aplikacji i wybierz polecenie **Właściwości**.  
  ![Strona pośrednicząca zaufanie kliknij prawym przyciskiem myszy nazwę aplikacji - screentshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  
3. Na karcie **punkty końcowe** w obszarze **Typ punktu końcowego**wybierz pozycję **Federacyjnych**.
4. W obszarze **Zaufane adres URL** wprowadź adres URL wprowadzony w obszarze **Zewnętrzny adres URL** serwera Proxy aplikacji, a następnie kliknij **przycisk OK**.  
  ![Dodawanie punktu końcowego — wartość adres URL zaufanej — zrzut ekranu](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="see-also"></a>Zobacz też

- [Publikowanie aplikacji za pomocą serwera Proxy aplikacji](active-directory-application-proxy-publish.md)
- [Włączyć logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)
- [Rozwiązywanie problemów, jakie masz z serwera Proxy aplikacji](active-directory-application-proxy-troubleshoot.md)
- [Włączanie aplikacji klientami w celu interakcji z aplikacjami serwera proxy](active-directory-application-proxy-native-client.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)
