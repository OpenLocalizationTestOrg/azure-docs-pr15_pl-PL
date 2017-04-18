<properties
    pageTitle="Azure Active Directory 2.0 — aplikacja systemu Android | Microsoft Azure"
    description="Jak utworzyć aplikację programu Android, która znaki w użytkownikom zarówno osobistego konta Microsoft i pracy lub szkolnych kont i połączeń interfejsu API wykresu za pomocą bibliotek innych firm."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Dodawanie logowania do aplikacji Android biblioteki innych firm za pomocą interfejsu API wykresu za pomocą punkt końcowy 2.0

Platforma Microsoft tożsamości używa otwartych standardów, takich jak uwierzytelnianie OAuth2 i łączenie OpenID. Deweloperzy mogą używać dowolnej biblioteki, które mają być Integracja z naszych usług. Aby ułatwić programistom naszej platformy za pomocą innych bibliotek, możemy napisanych kilka instruktaży podobny do tego celu zademonstrowania sposobu konfigurowania bibliotek innych firm nawiązywania połączenia z platformy Microsoft tożsamości. Większość bibliotek implementujących [Specyfikacja uwierzytelnianie OAuth2 RFC6749](https://tools.ietf.org/html/rfc6749) można nawiązać platformy Microsoft tożsamości.

Z tą aplikacją, który tworzy w tym instruktażu użytkownicy mogą zalogować się do swojej organizacji i wyszukaj się w organizacji za pomocą interfejsu API wykresu.

Jeśli znasz uwierzytelnianie OAuth2 lub połączyć OpenID duża część tej konfiguracji przykładowych może nie miały znaczenie dla Ciebie. Zalecamy, przeczytaj [protokoły 2.0 — przepływ kod autoryzacji 2.0 OAuth](active-directory-v2-protocols-oauth-code.md) tła.

> [AZURE.NOTE] Niektóre funkcje naszej platformy, które mają wyrażenia standardy uwierzytelnianie OAuth2 lub OpenID nawiązywanie połączenia, takie jak warunkowego dostępu i zarządzanie zasadami Intune, trzeba używać naszych Otwórz źródło Microsoft Azure tożsamości bibliotek.

Punkt końcowy 2.0 nie obsługuje wszystkich scenariuszy usługi Azure Active Directory i funkcji.

> [AZURE.NOTE] Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).


## <a name="download-the-code-from-github"></a>Pobieranie kodu z GitHub
Kod tego samouczka jest przechowywana [na GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2).  Aby wykonać opisane czynności, można [pobrać aplikację szkielet jako zip](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) lub klonowanie szkielet:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Możesz też po prostu można pobrać próbki i razu rozpocząć pracę:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Zarejestruj się w aplikacji
Tworzenie nowej aplikacji w [aplikacji portal rejestracji](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), lub wykonaj szczegółowe instrukcje opisane w temacie [jak zarejestrować aplikację z punktem końcowym 2.0](active-directory-v2-app-registration.md).  Upewnij się, że:

- Skopiuj **Identyfikator aplikacji** jest przypisany do aplikacji, ponieważ musisz je szybko.
- Dodawanie platformy **Mobile** dla aplikacji.

> Uwaga: Portal rejestracji aplikacji zawiera wartość **Przekieruj identyfikatora URI** . Jednak w tym przykładzie należy użyć wartości domyślnej `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>Pobieranie biblioteki innych firm NXOAuth2 i tworzenie obszaru roboczego

W tym instruktażu powinny zawierać OIDCAndroidLib z GitHub, czyli biblioteki uwierzytelnianie OAuth2 na podstawie kodu OpenID nawiązywanie połączenia z usługi Google. Wykonuje profilu natywnej aplikacji, a obsługuje punkt końcowy autoryzacji użytkownika. Są to wszystkie czynności, które musisz Integracja z platformy Microsoft tożsamości.

Klonowanie repo OIDCAndroidLib do komputera.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Konfigurowanie środowiska usługi Android Studio

1. Tworzenie nowego projektu Android Studio i zaakceptuj ustawienia domyślne w kreatorze.

    ![Tworzenie nowego projektu w Android Studio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Docelowy urządzeń z systemem Android](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Dodawanie działania na telefon komórkowy](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. Aby skonfigurować moduły projektu, Przenieś sklonowanym repo lokalizacja projektu. Możesz również utworzyć projektu i klonowanie go bezpośrednio do lokalizacji projektu.

    ![Moduły projektu](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Otwórz ustawienia moduły projektu, korzystając z menu kontekstowego lub za pomocą skrótu Ctrl + Alt + główn + S.

    ![Ustawienia modułów projektu](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Moduł aplikacji domyślnej należy usunąć, ponieważ chcesz tylko ustawienia kontenera projektu.

    ![Domyślny moduł aplikacji](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Importowanie modułów z sklonowanym repo do bieżącego projektu.

    ![Importowanie projektu gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![Utwórz nową stronę modułu](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Powtórz te kroki dla `oidlib-sample` modułu.

7. Sprawdź zależności oidclib na `oidlib-sample` modułu.

    ![zależności oidclib w module oidlib próbki](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Kliknij **przycisk OK** i poczekaj, aż gradle synchronizacji.

    Usługi settings.gradle powinna wyglądać podobnie do:

    ![Zrzut ekranu przedstawiający settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Tworzenie aplikacji przykładowych, aby upewnić się, że próbki działają poprawnie.

    Nie można używać to jeszcze z usługi Azure Active Directory. Firma Microsoft musisz najpierw skonfigurować kilka punktów końcowych. To, aby upewnić się, że nie masz problemy z systemem Android Studio przed rozpoczęciem możemy dostosowywania aplikacji przykładowych.

10. Tworzenie i uruchamianie `oidlib-sample` jako obiekt docelowy Android Studio.

    ![Informacje o postępie kompilacji oidlib próbki](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Usuwanie `app ` katalogu, w którym została wystawiona po usunięciu moduł z projektu, ponieważ Android Studio nie powoduje usunięcia go bezpieczeństwa.

    ![Struktura pliku, który zawiera katalog aplikacji](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Otwieranie menu **Edycja konfiguracji** , aby usunąć uruchamianie konfiguracji, która również pozostał po usunięciu moduł z projektu.

    ![Edytowanie menu konfiguracji](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![uruchamianie konfiguracji aplikacji](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>Konfigurowanie punkty końcowe próbki

Teraz, gdy masz `oidlib-sample` pomyślnie uruchomiona, edytowanie niektórych punktów końcowych do działało z usługą Azure Active Directory.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Konfigurowanie klienta, edytując plik oidc_clientconf.xml

1. Ponieważ używasz przepływów uwierzytelnianie OAuth2 tylko do uzyskać token i połączeń interfejsu API wykresu, Ustaw klienta uwierzytelnianie OAuth2 tylko. OIDC rozpocznie się w przykładzie później.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Skonfiguruj swój identyfikator klienta, otrzymany od portal rejestracji.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Konfigurowanie usługi przekierowania URI każdy z nich poniżej.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Konfigurowanie zakresach potrzebne dostęp do interfejsu API wykresu.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

`User.Read` Wartość w `oidc_scopes` pozwala na odczytywanie profil podstawowy podpisanych użytkownika.
Więcej informacji o wszystkie dostępne zakresy na [zakresów uprawnień programu Microsoft Graph](https://graph.microsoft.io/docs/authorization/permission_scopes)można znaleźć.

Jeśli chcesz wyjaśnienia dotyczące `openid` lub `offline_access` jako zakresy w OpenID połączenia, zobacz [protokoły 2.0 — przepływ kod autoryzacji OAuth 2.0](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Konfigurowanie punktów końcowych klienta, edytując plik oidc_endpoints.xml

- Otwórz `oidc_endpoints.xml` plik i wprowadzić następujące zmiany:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Te punkty końcowe nigdy nie należy zmieniać, jeśli korzystasz z uwierzytelnianie OAuth2 jako protokołów.

> [AZURE.NOTE]
Punkty końcowe dla `userInfoEndpoint` i `revocationEndpoint` nie są obecnie obsługiwane przez usługi Azure Active Directory. Pozostawienie tych z wartością domyślną example.com przypomnienie pojawi się że nie są one dostępne w próbce :-)


## <a name="configure-a-graph-api-call"></a>Konfigurowanie połączenia interfejsu API wykresu

- Otwórz `HomeActivity.java` plik i wprowadzić następujące zmiany:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

W tym miejscu prostego połączenia interfejsu API wykresu zwraca naszych informacje.

Znajdują się wszystkie zmiany, które należy wykonać. Uruchamianie `oidlib-sample` aplikacji i kliknij przycisk **Zaloguj**.

Po zostały pomyślnie uwierzytelniony, wybierz przycisk **Żądanie chronionych zasobów** do testowania połączeń API wykresu.

## <a name="get-security-updates-for-our-product"></a>Uzyskiwanie aktualizacji zabezpieczeń systemu nasze produkty

Zachęcamy do otrzymywanie powiadomień o zdarzeń, odwiedź witrynę [TechCenter zabezpieczeń](https://technet.microsoft.com/security/dd252948) i subskrybowania doradcze alertów zabezpieczeń.
