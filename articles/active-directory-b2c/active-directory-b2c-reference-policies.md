<properties
    pageTitle="Azure Active Directory B2C: Struktura zasad dotyczących Extensible | Microsoft Azure"
    description="Temat w ramach zasad extensible Azure Active Directory B2C i o sposobach tworzenia różnych typów zasad"
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

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory B2C: Struktura zasad dotyczących Extensible

## <a name="the-basics"></a>Podstawowe informacje

Extensible zasad ramach B2C usługi Azure Active Directory (Azure AD) jest siłę podstawowe usługi. Zasady pełne opisanie środowiska tożsamości dla klientów indywidualnych, takich jak zapisów, logowania lub profilu do edycji. Na przykład zapisów zasady pozwala kontrolować zachowanie, konfigurując następujące ustawienia:

- Typ konta (konta społecznościowych, takich jak Facebook lub lokalnych kont, takich jak adres e-mail) używanych przez klientów indywidualnych na utworzenie konta w aplikacji.
- Atrybuty (na przykład imię, kod pocztowy i rozmiar but) zostaną zebrane konsumenta podczas tworzenia konta.
- Użycie uwierzytelnianie wieloskładnikowe.
- Wygląd i działanie wszystkich stron zapisów.
- Informacje (które manifesty jako roszczeń w tokenu) czy po otrzymaniu kiedy zasady uruchamianie wykończenie.

Można utworzyć zasady wielu różnych typów w dzierżawie i używać ich w aplikacji, stosownie do potrzeb. Zasady może nastąpić w aplikacjach. Dzięki temu deweloperzy mogą definiować i modyfikować dla klientów indywidualnych tożsamości doświadczeń z minimalnymi lub nie zmiany ich kodu.

Zasady są dostępne za pośrednictwem interfejsu prosty Deweloper. Aplikacja uruchamia zasadę przy użyciu standardowych żądania uwierzytelniania HTTP (przekazanie parametru zasad w wezwaniu) i odbiera dostosowane token odpowiedzi. Na przykład tylko różnicę między żądania wywoływanie zapisów zasad i te wywoływanie zasady logowania to nazwa zasady używane w parametr ciągu kwerendy "p":

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Aby uzyskać więcej informacji na temat zasad dotyczących nadawców zobacz ten [wpis w blogu](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-policy"></a>Tworzenie zasad zapisów

Aby włączyć zapisów w swojej aplikacji, należy utworzyć zasadę zapisów. Tych zasad zawiera opis funkcji, które konsumentów zostanie obsłużone podczas tworzenia konta i zawartości tokenów wydawanych przez aplikację będą odbierane na pomyślne zapisy.

1. [Wykonaj poniższe czynności, aby przejść do karta funkcje B2C portalu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknij pozycję **zasady tworzenia konta**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. **Nazwa** Określa nazwę zapisów zasady używane przez aplikację. Na przykład "SiUp".
5. Kliknij pozycję **dostawcy tożsamości** i wybierz pozycję "Tworzenie konta E-mail". Opcjonalnie można też zaznaczyć dostawcy tożsamości społecznościowe, jeśli już skonfigurowane. Kliknij **przycisk OK**.
6. Kliknij pozycję **zapisów atrybutów**. W tym miejscu możesz wybrać atrybuty, które chcesz zebrać od konsumenta podczas tworzenia konta. Wybierz na przykład "Kraj/Region", "Nazwa wyświetlana" i "Kod pocztowy". Kliknij **przycisk OK**.
7. Kliknij pozycję **oświadczeń aplikacji**. W tym miejscu możesz wybrać oświadczeń, które mają być zwracane w tokeny wysyłane do aplikacji po pomyślnym aplecie. Wybierz na przykład "Nazwa wyświetlana", "Dostawcy tożsamości", "Kod pocztowy", "Użytkownika nowego" i "Identyfikator obiektu użytkownika".
8. Kliknij przycisk **Utwórz**. Należy zauważyć, że zasady właśnie utworzony jest wyświetlany jako "**B2C_1_SiUp**" ( **B2C\_1\_ ** fragment jest automatycznie dodawana) w karta **zasady tworzenia konta** .
9. Otwórz zasady, klikając pozycję "**B2C_1_SiUp**".
10. Wybierz pozycję "Contoso B2C aplikację" z listy rozwijanej **aplikacji** i `https://localhost:44321/` w **adres URL Odpowiedz i przekierowywanie identyfikatora URI** listy rozwijanej.
11. Kliknij przycisk **Uruchom teraz**. Zostanie otwarty w nowej karcie w przeglądarce, a może zostać uruchomiony za pośrednictwem obsługi dla klientów indywidualnych zapisywania się w aplikacji.

    > [AZURE.NOTE]
    Zajmuje minuty tworzenia zasad i aktualizacje zostały wprowadzone.

## <a name="create-a-sign-in-policy"></a>Tworzenie zasad logowania

Aby włączyć logowania w swojej aplikacji, należy utworzyć zasadę logowania. Tych zasad zawiera opis funkcji, które konsumentów zostaną umieszczone za pośrednictwem podczas logowania i zawartość tokenów, które otrzymają aplikacji na pomyślne dodatki logowania.

1. [Wykonaj poniższe czynności, aby przejść do karta funkcje B2C portalu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknij pozycję **zasady logowania**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. **Nazwa** Określa nazwę logowania zasady używane przez aplikację. Na przykład "SiIn".
5. Kliknij pozycję **dostawcy tożsamości** i wybierz pozycję "Lokalnego konta logowania". Opcjonalnie można też zaznaczyć dostawcy tożsamości społecznościowe, jeśli już skonfigurowane. Kliknij **przycisk OK**.
6. Kliknij pozycję **oświadczeń aplikacji**. W tym miejscu możesz wybrać oświadczeń, które mają być zwracane w tokeny wysyłane do aplikacji po pomyślnym obsługi logowania. Wybierz na przykład "Nazwa wyświetlana", "Dostawcy tożsamości", "Kod pocztowy" i "Identyfikator obiektu użytkownika". Kliknij **przycisk OK**.
7. Kliknij przycisk **Utwórz**. Należy zauważyć, że zasady utworzoną przed chwilą jest wyświetlany jako "**B2C_1_SiIn**" ( **B2C\_1\_ ** fragment jest automatycznie dodawana) w karta **zasady logowania** .
8. Otwórz zasady, klikając pozycję "**B2C_1_SiIn**".
9. Wybierz pozycję "Contoso B2C aplikację" z listy rozwijanej **aplikacje** i `https://localhost:44321/` w **adres URL Odpowiedz i przekierowywanie identyfikatora URI** listy rozwijanej.
10. Kliknij przycisk **Uruchom teraz**. Zostanie otwarty w nowej karcie w przeglądarce, a może zostać uruchomiony za pośrednictwem obsługi dla klientów indywidualnych logowania się do aplikacji.

    > [AZURE.NOTE]
    Zajmuje minuty tworzenia zasad i aktualizacje zostały wprowadzone.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Tworzenie zasad zapisów lub logowania

Tych zasad obsługuje zarówno dla klientów indywidualnych zapisów i logowania doświadczeń z jednej konfiguracji. Odbiorcy są doprowadziły do prawej ścieżki (zapisów lub logowania) w zależności od kontekstu. Zawiera również opis zawartości tokeny aplikacji zostanie wyświetlony po pomyślnym zapisy lub rejestrowaniu.  Przykładowy kod zasady zapisów lub logowania jest [dostępny tutaj](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Wykonaj poniższe czynności, aby przejść do karta funkcje B2C portalu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknij pozycję **zasadami zapisów lub logowania**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. **Nazwa** Określa nazwę zapisów zasady używane przez aplikację. Na przykład "SiUpIn".
5. Kliknij pozycję **dostawcy tożsamości** i wybierz pozycję "Tworzenie konta E-mail". Opcjonalnie można też zaznaczyć dostawcy tożsamości społecznościowe, jeśli już skonfigurowane. Kliknij **przycisk OK**.
6. Kliknij pozycję **zapisów atrybutów**. W tym miejscu możesz wybrać atrybuty, które chcesz zebrać od konsumenta podczas tworzenia konta. Wybierz na przykład "Kraj/Region", "Nazwa wyświetlana" i "Kod pocztowy". Kliknij **przycisk OK**.
7. Kliknij pozycję **oświadczeń aplikacji**. W tym miejscu możesz wybrać oświadczeń, które mają być zwracane w tokeny wysyłane do aplikacji po pomyślnym systemu zapisów lub logowania. Wybierz na przykład "Nazwa wyświetlana", "Dostawcy tożsamości", "Kod pocztowy", "Użytkownika nowego" i "Identyfikator obiektu użytkownika".
8. Kliknij przycisk **Utwórz**. Należy zauważyć, że zasady utworzoną przed chwilą jest wyświetlany jako "**B2C_1_SiUpIn**" ( **B2C\_1\_ ** fragment jest automatycznie dodawana) w karta **zasady zapisów lub logowania** .
9. Otwórz zasady, klikając pozycję "**B2C_1_SiUpIn**".
10. Wybierz pozycję "Contoso B2C aplikację" z listy rozwijanej **aplikacji** i `https://localhost:44321/` w **adres URL Odpowiedz i przekierowywanie identyfikatora URI** listy rozwijanej.
11. Kliknij przycisk **Uruchom teraz**. Zostanie otwarty w nowej karcie w przeglądarce, a można uruchamiać za pośrednictwem środowiska zapisów lub logowania dla klientów indywidualnych, zgodnie z konfiguracją.

    > [AZURE.NOTE]
    Zajmuje minuty tworzenia zasad i aktualizacje zostały wprowadzone.

## <a name="create-a-profile-editing-policy"></a>Tworzenie profilu edytowania zasad

Aby włączyć profilu w swojej aplikacji do edycji, będzie konieczne tworzenie profilu edytowania zasad. Tych zasad zawiera opis funkcji, które konsumentów zostanie obsłużone podczas edytowania profilu oraz zawartość tokenów wydawanych przez aplikację otrzymają od pomyślnego zakończenia.

1. [Wykonaj poniższe czynności, aby przejść do karta funkcje B2C portalu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknij przycisk **zasadami edytowanie profilu**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. **Nazwa** określa edycji Nazwa zasady używane przez aplikację profilu. Na przykład "SiPe".
5. Kliknij pozycję **dostawcy tożsamości** i wybierz pozycję "Adres E-mail". Opcjonalnie można też zaznaczyć dostawcy tożsamości społecznościowe, jeśli już skonfigurowane. Kliknij **przycisk OK**.
6. Kliknij przycisk **atrybuty profilu**. W tym miejscu możesz wybrać atrybuty, które konsumenta można wyświetlać i edytować. Wybierz na przykład "Kraj/Region", "Nazwa wyświetlana" i "Kod pocztowy". Kliknij **przycisk OK**.
7. Kliknij pozycję **oświadczeń aplikacji**. W tym miejscu możesz wybrać oświadczeń, które mają być zwracane w tokeny wysyłane do aplikacji po pomyślnym profilu środowisko edytowania. Wybierz na przykład "Nazwa wyświetlana" i "Kod pocztowy".
8. Kliknij przycisk **Utwórz**. Należy zauważyć, że zasady właśnie utworzony jest wyświetlany jako "**B2C_1_SiPe**" ( **B2C\_1\_ ** fragment jest automatycznie dodawana) w **profilu edytowanie zasad** karta.
9. Otwórz zasady, klikając pozycję "**B2C_1_SiPe**".
10. Wybierz pozycję "Contoso B2C aplikację" z listy rozwijanej **aplikacji** i `https://localhost:44321/` w **adres URL Odpowiedz i przekierowywanie identyfikatora URI** listy rozwijanej.
11. Kliknij przycisk **Uruchom teraz**. Zostanie otwarty w nowej karcie w przeglądarce, a może zostać uruchomiony przez profil obsługi dla klientów indywidualnych w aplikacji do edycji.

    > [AZURE.NOTE]
    Zajmuje minuty tworzenia zasad i aktualizacje zostały wprowadzone.
    
## <a name="create-a-password-reset-policy"></a>Tworzenie zasad resetowania hasła

Aby włączyć resetowania w swojej aplikacji szerokiego hasła, będzie konieczne tworzenie zasad resetowania hasła. Uwaga, że opcja określona do resetowania hasła na poziomie dzierżawy [w tym miejscu](active-directory-b2c-reference-sspr.md) jest nadal stosowane zasady logowania. Tych zasad zawiera opis funkcji, które konsumentów zostanie obsłużone podczas resetowania hasła i zawartość tokenów wydawanych przez aplikację otrzymają od pomyślnego zakończenia.

1. [Wykonaj poniższe czynności, aby przejść do karta funkcje B2C portalu Azure](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Kliknij przycisk **Resetuj hasło zasady**.
3. Kliknij przycisk **+ Dodaj** w górnej części karta.
4. **Nazwa** określa resetowania hasła nazwa zasady używane przez aplikację. Na przykład "SSPR".
5. Kliknij pozycję **dostawcy tożsamości** , a następnie zaznacz pole wyboru "Resetuj hasło przy użyciu adresu e-mail". Kliknij **przycisk OK**.
6. Kliknij pozycję **oświadczeń aplikacji**. W tym miejscu możesz wybrać oświadczeń, które mają być zwracane w tokeny wysyłane do aplikacji, po zresetowaniu hasła pomyślnego środowiska. Wybierz na przykład "Identyfikator obiektu użytkownika".
7. Kliknij przycisk **Utwórz**. Należy zauważyć, że zasady właśnie utworzony jest wyświetlany jako "**B2C_1_SSPR**" ( **B2C\_1\_ ** fragment jest automatycznie dodawana) w karta **zasady do resetowania hasła** .
8. Otwórz zasady, klikając pozycję "**B2C_1_SSPR**".
9. Wybierz pozycję "Contoso B2C aplikację" z listy rozwijanej **aplikacji** i `https://localhost:44321/` w **adres URL Odpowiedz i przekierowywanie identyfikatora URI** listy rozwijanej.
10. Kliknij przycisk **Uruchom teraz**. Zostanie otwarty w nowej karcie w przeglądarce, a można uruchamiać za pośrednictwem obsługi dla klientów indywidualnych resetowania hasła w aplikacji.

    > [AZURE.NOTE]
    Zajmuje minuty tworzenia zasad i aktualizacje zostały wprowadzone.

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Token, sesji i konfiguracji rejestracji jednokrotnej](active-directory-b2c-token-session-sso.md).
