<properties
    pageTitle="Jak włączyć publikowanie klientami aplikacje z aplikacjami serwera proxy | Microsoft Azure"
    description="Opisano, jak włączyć aplikacje klientami komunikowanie się z Azure AD aplikacji serwera Proxy łącznika bezpieczny dostęp zdalny do aplikacji lokalnej."
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

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Jak włączyć klientami aplikacje, aby wykonać czynność związaną z serwera proxy aplikacji

Azure Active Directory serwer Proxy aplikacji jest powszechnie używany do publikowania aplikacji przeglądarki, takie jak programu SharePoint, w programie Outlook Web Access i linii niestandardowej aplikacji biznesowych. Można również umożliwia za publikowanie klientami aplikacje, które różnią się w aplikacjach sieci web, ponieważ są instalowane na urządzeniu. Jest to dzięki obsłudze Azure AD wystawiony tokeny, które są wysyłane w standardowych nagłówków Autoryzuj HTTP.

![Relacja między użytkowników końcowych usługi Azure Active Directory i opublikowanych aplikacji](./media/active-directory-application-proxy-native-client/richclientflow.png)

Zaleca się publikowania tych aplikacji jest użycie Azure AD uwierzytelniania biblioteki, która odpowiada za wykonywanie wszystkich problemów uwierzytelniania i obsługuje wiele środowiska innego klienta. Serwer Proxy aplikacji jest dopasowywana do [Natywnej aplikacji do scenariusza interfejs API sieci Web](active-directory-authentication-scenarios.md#native-application-to-web-api). Proces realizacji tego zadania jest w następujący sposób:

## <a name="step-1-publish-your-application"></a>Krok 1: Publikowanie aplikacji

Publikowanie aplikacji serwera proxy, jak w przypadku dowolnej innej aplikacji, Przypisz użytkowników i nadać im premium i podstawowe licencji. Aby uzyskać więcej informacji, zobacz [Publikowanie aplikacji za pomocą serwera Proxy aplikacji](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Krok 2: Konfigurowanie aplikacji

Skonfigurować aplikację natywne w następujący sposób:

1. Zaloguj się do portalu klasyczny Azure.
2. Wybierz ikonę usługi Active Directory w menu po lewej stronie, a następnie wybierz pozycję katalogu.
3. W górnym menu kliknij pozycję **aplikacje**. Jeśli aplikacji nie zostały dodane do katalogu, ta strona będą wyświetlane tylko łącze **Dodaj aplikację** . Kliknij łącze lub Alternatywnie możesz kliknąć przycisk **Dodaj** na pasku poleceń.
4. Na stronie **co chcesz zrobić** kliknij łącze, aby **dodać aplikację, którą rozwija się w mojej organizacji**.
5. Na stronie **Przekaż nam o aplikacji** Określ nazwę dla aplikacji, a następnie wybierz **aplikację Native client**. Kliknij ikonę strzałki, aby kontynuować.
6. Na stronie **informacje o aplikacji** podaj **Przekierowywać identyfikatora URI** dla aplikacji klientami, a następnie kliknij znacznik wyboru, aby zakończyć.

Dodano aplikacji, a następnie nastąpi przekierowanie do strony Szybki Start dla aplikacji.

## <a name="step-3-grant-access-to-other-applications"></a>Krok 3: Udzielanie dostępu do innych aplikacji

Włącz natywnej aplikacji udostępnić do innych aplikacji w katalogu:

1. W górnym menu kliknij pozycję **aplikacje**, wybierz pozycję Nowy natywnej aplikacji i kliknij przycisk **Konfiguruj**.
2. Przewiń w dół do sekcji **uprawnienia do innych aplikacji** . Kliknij przycisk **Dodaj aplikację** i wybierz aplikację serwera proxy, który chcesz udzielić dostępu natywnej aplikacji do, a następnie kliknij znacznik wyboru w prawym dolnym rogu. Z menu rozwijanego **Delegowane uprawnienia** zaznacz nowe uprawnienie.

![Uprawnienia do innych aplikacji zrzut ekranu — Dodawanie aplikacji](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Krok 4: Edytowanie biblioteki uwierzytelniania usługi Active Directory

Edytowanie kodu natywnej aplikacji w kontekście uwierzytelniania z Active Directory uwierzytelniania biblioteki (ADAL) do są następujące:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

Zmienne zastępuje się następująco:

- **TenantId** można znaleźć w GUID w adresie URL strony **konfiguracji** aplikacji, bezpośrednio po "/ katalogu /".
- Adres URL fronton wprowadzone w aplikacji serwera Proxy i można znaleźć na stronie **konfiguracji** serwera proxy aplikacji jest używany adres **URL serwera sieci Web** .
- **Identyfikator klienta** natywnej aplikacji można znaleźć na stronie **Konfigurowanie** natywnej aplikacji.
- **Przekierowywanie URI natywnej aplikacji** można znaleźć na stronie **Konfigurowanie** natywnej aplikacji.

![Zrzut ekranu strony Konfigurowanie nowej natywnej aplikacji](./media/active-directory-application-proxy-native-client/new_native_app.png)

Aby uzyskać więcej informacji o procesie natywnej aplikacji zobacz [natywnej aplikacji do interfejsu API sieci web](active-directory-authentication-scenarios.md#native-application-to-web-api).


## <a name="see-also"></a>Zobacz też

- [Publikowanie aplikacji przy użyciu własnej nazwy domeny](active-directory-application-proxy-custom-domains.md)
- [Włączanie dostępu warunkowego](active-directory-application-proxy-conditional-access.md)
- [Praca z aplikacjami pamiętać o oświadczeniach](active-directory-application-proxy-claims-aware-apps.md)
- [Włączyć logowania jednokrotnego](active-directory-application-proxy-sso-using-kcd.md)

Aby uzyskać najnowsze informacje i aktualizacje zapoznaj się z [serwera Proxy aplikacji blogu](http://blogs.technet.com/b/applicationproxyblog/)
