<properties
    pageTitle="Wprowadzenie do programu Microsoft Graph i Azure Active Directory ochronę tożsamości | Microsoft Azure"
    description="Wprowadzenie do kwerend programu Microsoft Graph dla listy wydarzeń ryzyka i skojarzone z nim informacje z usługi Azure Active Directory."
    services="active-directory"
    keywords="ochrona tożsamości usługi Azure active directory, zdarzenia ryzyka, luka w zabezpieczeniach zasad zabezpieczeń programu Microsoft Graph"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Wprowadzenie do programu Microsoft Graph i Azure Active Directory ochronę tożsamości

Program Microsoft Graph jest ujednolicony końcowy interfejsu API firmy Microsoft i dla użytkowników domowych [Azure Active Directory ochronę tożsamości i](active-directory-identityprotection.md) interfejsów API. Nasze pierwszego interfejsu API **identityRiskEvents**umożliwia kwerend programu Microsoft Graph dla listy [wydarzeń ryzyka](active-directory-identityprotection-risk-events-types.md) i skojarzone z nim informacje. W tym artykule otrzymuje zostanie uruchomiona kwerenda ten interfejs API. W długości wprowadzenie, pełną dokumentację i dostęp do Eksploratora wykresu, zobacz [witrynę programu Microsoft Graph](https://graph.microsoft.io/).

Istnieją trzy kroki w celu uzyskiwania dostępu do danych ochronę tożsamości za pomocą programu Microsoft Graph:

1. Dodawanie aplikacji przy użyciu hasła klienta. 

2. Użyj tego hasła i kilka innych rodzajów informacji do uwierzytelnienia do programu Microsoft Graph, w którym będziesz otrzymywać token uwierzytelniania. 

3. Ten token umożliwia tworzenie żądania do punktu końcowego interfejsu API i wrócić tożsamości ochrony danych.

Przed rozpoczęciem pracy, musisz:

- Uprawnienia administratora, aby utworzyć aplikację w Azure AD
- Nazwa domeny Twojej dzierżawy (na przykład contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Dodawanie aplikacji przy użyciu hasła klienta


1. [Zaloguj się](https://manage.windowsazure.com) do portalem klasyczny Azure jako administrator. 

1. W okienku nawigacji po lewej stronie wybierz polecenie **Usługi Active Directory**. 

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. Na liście **katalogu** zaznacz katalogu, dla której chcesz włączyć integracji katalogów.

3. W menu u góry kliknij pozycję **aplikacje**.

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Kliknij przycisk **Dodaj** w dolnej części strony.

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. W oknie dialogowym **co chcesz zrobić** kliknij przycisk **Dodaj aplikację, którą rozwija się w mojej organizacji**.

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. W oknie dialogowym **Przekaż nam informacje o aplikacji** wykonaj następujące czynności:

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    . W polu tekstowym **Nazwa** wpisz nazwę aplikacji (np.: AADIP ryzyka zdarzenia interfejsu API aplikacji).

    b. Jako **Typ**wybierz pozycję **aplikacji i/lub sieci Web interfejsu API sieci Web**.

    c. Kliknij przycisk **Dalej**.


5. W oknie dialogowym **Właściwości aplikacji** wykonaj następujące czynności:

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    . W polu tekstowym **Adres URL logowania jednokrotnego** wpisz `http://localhost`.

    b. W polu tekstowym **Identyfikator URI identyfikator aplikacji** wpisz `http://localhost`.

    c. Kliknij pozycję **pełne**.


Teraz można skonfigurować aplikację.

![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Zezwolić aplikacji za pomocą interfejsu API


1. Na stronie aplikacji, w menu u góry kliknij przycisk **Konfiguruj**. 

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. W sekcji **uprawnienia do innych aplikacji** kliknij przycisk **Dodaj aplikację**.

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. W oknie dialogowym **uprawnienia do innych aplikacji** wykonaj następujące czynności:

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    . Wybierz pozycję **Microsoft Graph**.

    b. Kliknij pozycję **pełne**.


1. Kliknij pozycję **uprawnienia aplikacji: 0**, a następnie wybierz pozycję **czytać wszystkie informacje zdarzenia ryzyka tożsamości**.

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. Kliknij pozycję **Zapisz** u dołu strony.

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Uzyskiwanie dostępu do klucza

1. Na stronie aplikacji w sekcji **kluczy** wybierz roczną jako czas trwania.

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. Kliknij pozycję **Zapisz** u dołu strony.

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. w sekcji klawisze skopiować wartość do nowo utworzony klucz, a następnie wklej go w bezpiecznym miejscu.

    ![Tworzenie aplikacji](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Jeśli zgubisz klucz ten będzie trzeba powróć do tej sekcji i Utwórz nowy klucz. Zachowaj ten klucz tajny: każda osoba, która jest on uzyskać dostęp do danych.


1. W sekcji **Właściwości** Skopiuj **Identyfikator klienta**, a następnie wklej go w bezpiecznym miejscu. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Uwierzytelnianie do programu Microsoft Graph i kwerendy API zdarzenia ryzyka tożsamości

W tym momencie powinien mieć:

- Identyfikator klienta, który został skopiowany powyżej

- Klucz, który został skopiowany powyżej

- Nazwa domeny dzierżawcy usługi


Do uwierzytelnienia, Wyślij żądanie wpis `https://login.microsoft.com` z następujących parametrów w treści:

- grant_type: "**client_credentials**"

- Zasób: "**https://graph.microsoft.com**"

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] Należy podać wartości **client_id** i parametru **client_secret** .

Jeśli powiedzie, to zwraca token uwierzytelniania.  
Aby nawiązać połączenie z interfejsu API, należy utworzyć nagłówek z następującym parametrem:

    `Authorization`=”<token_type> <access_token>"


Podczas uwierzytelniania, typ tokenu i token dostępu można znaleźć w zwracanych token.

Wyślij ten nagłówek jako żądanie następujący adres URL interfejsu API:`https://graph.microsoft.com/beta/identityRiskEvents`

Odpowiedzi, w przypadku powodzenia to zbiór zdarzenia ryzyka tożsamości i skojarzonych danych w formacie OData JSON, który można przeanalizować i obsługiwana, jak odpowiednio do potrzeb.

Poniżej przedstawiono przykładowy kod do uwierzytelniania i nawiązywania połączeń z interfejsu API, przy użyciu programu Powershell.  
Po prostu dodać swój identyfikator klienta klucza i dzierżawa domeny.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Następne kroki

Gratulacje, właśnie utworzony pierwszego połączenia do programu Microsoft Graph!  
Teraz możesz kwerendy zdarzenia ryzyka tożsamości i korzystania z danych, jednak odpowiednio do potrzeb.

Aby dowiedzieć się więcej na temat programu Microsoft Graph i tworzyć aplikacje przy użyciu interfejsu API wykresu, zaznacz [dokumentacji](https://graph.microsoft.io/docs) i bardziej w [witrynie programu Microsoft Graph](https://graph.microsoft.io/). Upewnij się również utworzyć zakładkę strony [ochronę Azure AD tożsamości interfejsu API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) , która zawiera listę wszystkich interfejsów API ochrony tożsamości, które są dostępne na wykresie. Przy wdrażaniu nowe sposoby pracy z ochronę tożsamości za pośrednictwem interfejsu API, zostaną one wyświetlone na tej stronie.


## <a name="additional-resources"></a>Dodatkowe zasoby

- [Ochrona tożsamości Azure Active Directory](active-directory-identityprotection.md)

- [Typy zdarzeń ryzyka wykryte przez Azure Active Directory ochronę tożsamości](active-directory-identityprotection-risk-events-types.md)

- [Program Microsoft Graph](https://graph.microsoft.io/)

- [Omówienie programu Microsoft Graph](https://graph.microsoft.io/docs)

- [Podstawy usługi ochrony Azure AD tożsamości](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
