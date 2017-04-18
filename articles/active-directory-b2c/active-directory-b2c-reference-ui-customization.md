<properties
    pageTitle="Azure Active Directory B2C: Dostosowania interfejsu użytkownika | Microsoft Azure"
    description="Temat Azure Active Directory B2C z funkcjami dostosowania interfejsu użytkownika"
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
    ms.date="07/25/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-customize-the-azure-ad-b2c-user-interface-ui"></a>Azure Active Directory B2C: Dostosowywanie Azure AD B2C interfejsu użytkownika (UI)

Środowisko użytkownika jest najważniejsze w aplikacji dla klientów indywidualnych skierowaną. Jest to różnica między dobre aplikacji i profesjonalnych jedną oraz między jedynie aktywne konsumentów i naprawdę zajętym z nich. B2C Azure Active Directory (Azure AD) umożliwia dostosowywanie dla klientów indywidualnych zapisów, logowania (*Zobacz poniższą uwagę*), edycji, profilu i stron z kontrola piksel idealny do resetowania hasła.

> [AZURE.NOTE]
Obecnie lokalnego konta logowania stron, hasło accompaning Resetowanie stron i weryfikacji wiadomości e-mail można dostosować tylko przy użyciu [funkcji znakowania firmy](../active-directory/active-directory-add-company-branding.md) , a nie przez mechanizmy opisane w tym artykule.

W tym artykule odczyta informacje:

- Funkcja dostosowania interfejsu użytkownika strony.
- Narzędzia, które mogą pomóc testowanie funkcji dostosowania interfejsu użytkownika strony przy użyciu naszych zawartości próbki.
- Podstawowe elementy interfejsu użytkownika w każdego typu strony.
- Najważniejsze wskazówki dotyczące wykonywania tej funkcji.

## <a name="the-page-ui-customization-feature"></a>Funkcja dostosowywania strony interfejsu użytkownika

Za pomocą funkcji dostosowywania strony interfejsu użytkownika można dostosować wygląd i działanie indywidualnych zapisów, logowania, Resetuj hasła i edytowanie profilu stron (przez skonfigurowanie [zasad](active-directory-b2c-reference-policies.md)). Osób korzystających ze uzyskuje Bezproblemowa środowiska podczas nawigowania między do aplikacji i stron obsługiwanych przez Azure AD B2C.

W przeciwieństwie do innych usług miejsce, w którym opcji interfejsu użytkownika są ograniczone lub są dostępne za pośrednictwem interfejsów API tylko Azure AD B2C używa Nowoczesny (i prostsze) podejście do dostosowywania strony składników interfejsu użytkownika.

Poniżej opisano, jak to działa: Azure AD B2C kod w przeglądarce usługi dla klientów indywidualnych, korzysta z podejście nowoczesny o nazwie [Udostępnianie zasobów między Origin (CORS)](http://www.w3.org/TR/cors/) aby załadować zawartości z adresu URL określające zasady. Możesz określić innych adresów URL dla różnych stron. Kod scala elementów interfejsu użytkownika z Azure AD B2C z zawartością załadowana z adresu URL, a zostanie wyświetlona strona z klientem. Wszystko, co należy zrobić to:

1. Tworzenie zawartości z poprawnego HTML5 `<div id="api"></div>` elementu (musi zostać elementem pustym) znajduje się gdzieś w `<body>`. To miejsce, w którym została wstawiona zawartość Azure AD B2C znaczników elementu.
2. Udostępniać zawartość dla punktu końcowego HTTPS (z CORS dozwolone).
3. Styl elementów interfejsu użytkownika, które Azure AD B2C zostanie wstawiony w.

## <a name="test-out-the-ui-customization-feature"></a>Przetestowanie funkcji dostosowanie interfejsu użytkownika

Jeśli użytkownik chce wypróbować funkcję dostosowania interfejsu użytkownika przy użyciu nasz przykładowy kod HTML i arkuszy CSS zawartości, udostępniamy możesz [narzędziu simple Pomocnik](active-directory-b2c-reference-ui-customization-helper-tool.md) przekazywanie i konfiguruje zawartość przykładowa w magazynie obiektów Blob platformy Azure.

> [AZURE.NOTE]
Możesz hostować dowolne miejsce zawartości interfejsu użytkownika: na serwerach sieci web, sieci CDN, AWS S3, systemów udostępniania plików itp. Jak zawartość jest hostowana na publicznie dostępne końcowego HTTPS (z CORS dozwolone), są możesz kontynuować. Użyto magazyn obiektów Blob platformy Azure tylko do celów opisowy.

### <a name="the-most-basic-html-content-for-testing"></a>Najprostszym zawartości HTML do celów testowych

Jak pokazano poniżej jest najbardziej podstawowe HTML zawartości, której można przetestować tę funkcję. Użyj tego samego narzędzia Pomocnik znajdujących się we wcześniejszej przekazywanie i skonfiguruj tej zawartości w magazynie obiektów Blob platformy Azure. Następnie można sprawdzić, czy przyciski podstawowych, stylized i polami formularza na każdej stronie są wyświetlane i funkcjonalne.

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>!Add your title here!</title>
    </head>
    <body>
        <div id="api"></div>    <!-- IMP: This element is intentionally empty; don't enter anything here -->
    </body>
</html>

```

## <a name="the-core-ui-elements-in-each-type-of-page"></a>Podstawowe elementy interfejsu użytkownika w każdego typu strony

W poniższych sekcjach znajdują się przykłady fragmenty HTML5, które scala Azure AD B2C `<div id="api"></div>` elementu znajdującego się w treści. **Nie należy wstawiać tych fragmenty zawartości HTML 5.** Usługa Azure AD B2C wstawia je w czasie wykonywania. W tych przykładach służy do projektowania własne arkusze stylów.

### <a name="azure-ad-b2c-content-inserted-into-the-identity-provider-selection-page"></a>Azure zawartości AD B2C wstawione do "tożsamości dostawcy zaznaczenia strona"

Ta strona zawiera listę dostawców tożsamości, których użytkownik może wybierać podczas tworzenia konta lub logowania. Są to dostawców tożsamości społecznościowych, takich jak Facebook i Google + lub lokalnych kont (na podstawie nazwa użytkownika lub adres e-mail).

```HTML

<div id="api" data-name="IdpSelections">
    <div class="intro">
         <p>Sign up</p>
    </div>

    <div>
        <ul>
            <li>
                <button class="accountButton" id="FacebookExchange">Facebook</button>
            </li>
            <li>
                <button class="accountButton" id="GoogleExchange">Google+</button>
            </li>
            <li>
                <button class="accountButton" id="SignUpWithLogonEmailExchange">Email</button>
            </li>
        </ul>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-local-account-sign-up-page"></a>Azure zawartości AD B2C wstawione do "Strona zapisów lokalnego konta"

Ta strona zawiera formularz zapisów, że użytkownik ma wypełnić podczas tworzenia konta dla lokalnego konta opartego na adres e-mail lub nazwę użytkownika. Formularz może zawierać różne kontrolki wprowadzania danych, takie jak pola do wprowadzania tekstu, polu wprowadzania hasła, przycisk opcji, pola listy rozwijanej wybierz pozycję pojedyncze i wybrać wiele pól wyboru.

```HTML

<div id="api" data-name="SelfAsserted">
    <div class="intro">
        <p>Create your account by providing the following details</p>
    </div>

    <div id="attributeVerification">
        <div class="errorText" id="passwordEntryMismatch" style="display: none;">The password entry fields do not match. Please enter the same password in both fields and try again.</div>
        <div class="errorText" id="requiredFieldMissing" style="display: none;">A required field is missing. Please fill out all required fields and try again.</div>
        <div class="errorText" id="fieldIncorrect" style="display: none;">One or more fields are filled out incorrectly. Please check your entries and try again.</div>
        <div class="errorText" id="claimVerificationServerError" style="display: none;"></div>
        <div class="attr" id="attributeList">
            <ul>
                <li>
                    <div class="attrEntry validate">
                        <div>
                            <div class="verificationInfoText" id="email_intro" style="display: inline;">Verification is necessary. Please click Send button.</div>
                            <div class="verificationInfoText" id="email_info" style="display:none">Verification code has been sent to your inbox. Please copy it to the input box below.</div>
                            <div class="verificationSuccessText" id="email_success" style="display:none">E-mail address verified. You can now continue.</div>
                            <div class="verificationErrorText" id="email_fail_retry" style="display:none">Incorrect code, try again.</div>
                            <div class="verificationErrorText" id="email_fail_no_retry" style="display:none">Exceeded number of retries you need to send new code.</div>
                            <div class="verificationErrorText" id="email_fail_server" style="display:none">Server error, please try again</div>
                            <div class="verificationErrorText" id="email_incorrect_format" style="display:none">Incorect format.</div>
                        </div>

                    <div class="helpText show">This information is required</div>
                        <label>Email</label>
                        <input id="email" class="textInput" type="text" placeholder="Email" required="" autofocus=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Email address that can be used to contact you.');" class="tiny">What is this?</a>

                    <div class="buttons verify" claim_id="email">
                        <div id="email_ver_wait" class="working" style="display: none;"></div>
                            <label id="email_ver_input_label" for="email_ver_input" style="display: none;">Verification code</label>
                            <input id="email_ver_input" type="text" placeholder="Verification code" style="display:none">
                            <button id="email_ver_but_send" class="sendButton" type="button" style="display: inline;">Send verification code</button>
                            <button id="email_ver_but_verify" class="verifyButton" type="button" style="display:none">Verify code</button>
                            <button id="email_ver_but_resend" class="sendButton" type="button" style="display:none">Send new code</button>
                            <button id="email_ver_but_edit" class="editButton" type="button" style="display:none">Change e-mail</button>
                            <button id="email_ver_but_default" class="defaultButton" type="button" style="display:none">Default</button>
                        </div>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ " ( ) ; .This information is required</div>
                        <label>Enter password</label>
                        <input id="password" class="textInput" type="password" placeholder="Enter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title="8-16 characters, containing 3 out of 4 of the following: Lowercase characters, uppercase characters, digits (0-9), and one or more of the following symbols: @ # $ % ^ &amp; * - _ + = [ ] { } | \ : ' , ? / ` ~ &quot; ( ) ; ." required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Enter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"> This information is required</div>
                        <label>Reenter password</label>
                        <input id="reenterPassword" class="textInput" type="password" placeholder="Reenter password" pattern="^((?=.*[a-z])(?=.*[A-Z])(?=.*\d)|(?=.*[a-z])(?=.*[A-Z])(?=.*[^A-Za-z0-9])|(?=.*[a-z])(?=.*\d)(?=.*[^A-Za-z0-9])|(?=.*[A-Z])(?=.*\d)(?=.*[^A-Za-z0-9]))([A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~&quot;();!]|\.(?!@)){8,16}$" title=" " required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Reenter password');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Name</label>
                        <input id="displayName" class="textInput" type="text" placeholder="Name" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your display name.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Gender</label>
                        <input id="extension_Gender_F" name="extension_Gender" type="radio" value="F" autofocus="">
                        <label for="extension_Gender_F">Female</label>
                        <input id="extension_Gender_M" name="extension_Gender" type="radio" value="M">
                        <label for="extension_Gender_M">Male</label>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>Loyalty number</label>
                        <input id="extension_MemNum" class="textInput" type="text" placeholder="Loyalty number"><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Membership number');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText"></div>
                        <label>State</label>
                        <select class="dropdown_single" id="state">
                            <option style="display:none" disabled="disabled" value="placeholder" selected="">State</option>
                            <option value="WA">Washington</option>
                            <option value="NY">New York</option>
                            <option value="CA">California</option>
                        </select>
                        <a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('Your residential state or province.');" class="tiny">What is this?</a>
                    </div>
                </li>
                <li>
                    <div class="attrEntry">
                        <div class="helpText">This information is required</div>
                        <label>Zip code</label>
                        <input id="postalCode" class="textInput" type="text" placeholder="Zip code" required=""><a href="javascript:void(0)" onclick="selfAssertedClient.showHelp('The postal code of your address.');" class="tiny">What is this?</a>
                    </div>
                </li>
            </ul>
        </div>
        <div class="buttons"> <button id="continue" disabled="">Create</button> <button id="cancel">Cancel</button></div>
    </div>
    <div class="verifying-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="verifying_blurb"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-social-account-sign-up-page"></a>Azure zawartości AD B2C wstawione do "" Social zapisów stronę konta"

Ta strona zawiera formularz zapisów, zawierającą konsumenta do wypełnienia podczas logowania przy użyciu istniejącego konta od dostawcy tożsamości społecznościowych, takich jak Facebook lub Google +. Ta strona jest podobna do lokalnego konta zapisów strony (jak pokazano w poprzedniej sekcji) z wyjątkiem pola hasła.

### <a name="azure-ad-b2c-content-inserted-into-the-unified-sign-up-or-sign-in-page"></a>Azure zawartości AD B2C wstawione do "ujednolicony zapisów lub logowania strony"

Ta strona obsługuje zarówno zapisów i logowania konsumentów, którzy mogą korzystać z dostawcy tożsamości społecznościowych, takich jak Facebook lub Google + lub konta lokalne.

```HTML

<div id="api" data-name="Unified">
        <div class="social" role="form">
               <div class="intro">
                       <h2>Sign in with your social account</h2>
               </div>
               <div class="options">
                       <div><button class="accountButton firstButton" id="MicrosoftAccountExchange" tabindex="1">msa</button></div>
                       <div><button class="accountButton" id="FacebookExchange" tabindex="1">fb</button></div>
               </div>
        </div>
        <div class="divider">
               <h2>OR</h2>
        </div>
        <div class="localAccount" role="form">
               <div class="intro">
                       <h2>Sign in with your existing account</h2>
               </div>
               <div class="error pageLevel" aria-hidden="true" style="display: none;">
                       <p role="alert"></p>
               </div>
               <div class="entry">
                       <div class="entry-item">
                               <label for="logonIdentifier">Email Address</label> 
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="email" id="logonIdentifier" name="LogonIdentifier" pattern="^[a-zA-Z0-9.!#$%&amp;’*+/=?^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" placeholder="LogonIdentifier" value="" tabindex="1">
                       </div>
                       <div class="entry-item">
                               <div class="password-label"> <label for="password">Password</label><a id="forgotPassword" tabindex="2">Forgot your password?</a></div>
                               <div class="error itemLevel" aria-hidden="true" style="display: none;">
                                      <p role="alert"></p>
                               </div>
                               <input type="password" id="password" name="Password" placeholder="Password" tabindex="1">
                       </div>
                       <div class="working"></div>
                       <div class="buttons"> <button id="next" tabindex="1">Sign in</button> </div>
               </div>
               <div class="divider">
                       <h2>OR</h2>
               </div>
               <div class="create">
                       <p>Don't have an account?<a id="createAccount" tabindex="1">Sign up now</a> </p>
               </div>
        </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-multi-factor-authentication-page"></a>Azure zawartości AD B2C wstawione do "Strona uwierzytelnianie wieloskładnikowe"

Na tej stronie użytkowników, można sprawdzić swoje numery telefonów (przy użyciu tekstu lub głosu) podczas tworzenia konta lub logowania.

```HTML

<div id="api" data-name="Phonefactor">
    <div id="phonefactor_initial">
        <div class="intro">
            <p>Enter a number below that we can send a code via SMS or phone to authenticate you.</p>
        </div>
        <div class="errorText" id="errorMessage" style="display:none"></div>
        <div class="phoneEntry" id="phoneEntry">
            <div class="phoneNumber">
                <select id="countryCode" style="display:inline-block">
                    <option value="+93">Afghanistan (+93)</option>
                    <!-- Not all country codes listed -->
                    <option value="+44">United Kingdom (+44)</option>
                    <option value="+1" selected="">United States (+1)</option>
                    <!-- Not all country codes listed -->
                </select>
            </div>
            <div class="phoneNumber">
                <input type="text" id="localNumber" style="display:inline-block" placeholder="Phone number">
            </div>
        </div>
        <div id="codeVerification" style="display:none">
            <div>
                <p>Enter your verification code below, or <button id="retryCode" class="textButton">send a new code</button></p>
                <input type="text" id="verificationCode" placeholder="Verification code">
            </div>
        </div>
        <div class="buttons">
            <button id="verifyCode" class="sendInitialCodeButton">Send Code</button>
            <button id="verifyPhone" style="display:inline-block">Call Me</button>
            <button id="cancel" style="display:inline-block">Cancel</button>
        </div>
    </div>
    <div class="dialing-modal">
        <div class="preloader"> <img src="https://login.microsoftonline.com/static/img/win8loader.gif" alt="Please wait"></div>
        <div id="dialing_blurb"></div><div id="dialing_number"></div>
    </div>
</div>

```

### <a name="azure-ad-b2c-content-inserted-into-the-error-page"></a>Azure zawartości AD B2C wstawione do "" Błąd strony"

```HTML

<div id="api" class="error-page-content" data-name="GlobalException">
    <h2>Sorry, but we're having trouble signing you in.</h2>
    <div class="error-page-help">We track these errors automatically, but if the problem persists feel free to contact us. In the meantime, please try again.</div>
    <div class="error-page-messagedetails">Your administrator hasn't provided any contact details.</div>
    <div class="error-page-messagedetails">
        <div class="error-page-correlationid">Correlation ID:1c4f0397-c6e4-4afe-bf74-42f488f2f15f</div>
        <div>Timestamp:2015-09-14 23:22:35Z</div>
        <div class="error-page-detail">AADB2C90065: A B2C client-side error 'Access is denied.' has occurred requesting the remote resource.</div>
    </div>
</div>

```

## <a name="things-to-remember-when-building-your-own-content"></a>Zagadnienia, które należy uwzględnić podczas tworzenia własnych zawartości

Jeśli planujesz korzystanie z funkcji dostosowania interfejsu użytkownika strony, przejrzyj następujące wskazówki:

- Nie skopiuj zawartość domyślną Azure AD B2C i modyfikowania go. Najlepiej tworzenia zawartości HTML5 od podstaw i używanie zawartości domyślnej jako odwołania.
- Na wszystkich stronach (z wyjątkiem strony błędu) obsługiwanych przez logowania, zapisywania i edytowanie profilu zasad, arkusze stylów, które są udostępniane będą musiały zastępują domyślne arkusze stylów, które dodajemy do tych stron w <head> fragmenty. Na wszystkich stronach obsługiwane przez zakładanie konta lub logowania i zasad resetowania hasła oraz stron błędów dla wszystkich zasad, konieczne będzie podanie wszystkie style samodzielnie.
- Ze względów bezpieczeństwa firma Microsoft nie jest możliwe do uwzględnienia dowolnego kodu JavaScript w zawartości. Większość potrzebnych powinny być dostępne po ich otrzymaniu. W przeciwnym razie żądanie nowe funkcje za pomocą [Głosu użytkownika](http://feedback.azure.com/forums/169401-azure-active-directory) .
- Obsługiwanych wersji przeglądarek:
    - Program Internet Explorer 11
    - Program Internet Explorer 10
    - Internet Explorer 9 (ograniczone)
    - Internet Explorer 8 (ograniczone)
    - Google Chrome 43.0
    - Google Chrome 42.0
    - Mozilla Firefox 38.0
    - Mozilla Firefox 37.0
