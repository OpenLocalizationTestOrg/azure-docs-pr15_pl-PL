<properties 
    pageTitle="Sposobu delegowania użytkownika subskrypcji produktu i rejestracji" 
    description="Dowiedz się, jak pełnomocnika subskrypcji produktu i rejestracji użytkownika do innej strony w zarządzaniu interfejsu API Azure." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Sposobu delegowania użytkownika subskrypcji produktu i rejestracji

Delegowanie pozwala do obsługi Deweloper logowania w i sign-zwiększenia i subskrypcji do produktów, a nie przy użyciu wbudowanych funkcji w portalu Deweloper istniejącą witrynę sieci Web. Dzięki temu witryny sieci Web do własnych danych użytkownika i sprawdzana poprawność kroki w sposób niestandardowy.

## <a name="delegate-signin-up"> </a>Deweloper delegowanie logowania i zapisów

Do pełnomocnika Deweloper logowania i zapisywania się do istniejącej witryny sieci Web należy utworzyć punkt końcowy specjalne delegowanie w witrynie działająca jako punkt wejścia takie żądanie zainicjowane za pomocą portalu zarządzania interfejsu API Deweloper.

Ostateczne przepływ pracy będzie w następujący sposób:

1. Deweloper klika łącze logowania lub zapisywania się w portalu zarządzania interfejsu API Deweloper
2. Przeglądarka jest przekierowywana do punktu końcowego delegowania
3. Delegowanie punkt końcowy w zamian przekierowuje lub przedstawia pytaniem użytkownika w celu logowania lub zapisów interfejsu użytkownika
4. W przypadku powodzenia użytkownik jest przekierowany z powrotem do strony portalu Deweloper interfejsu API zarządzania uruchomienia z


Aby rozpocząć, Przejdźmy pierwszej konfiguracji zarządzania interfejsu API do kierowania żądań za pośrednictwem punkt końcowy delegowanie. W portalu zarządzania interfejsu API programu publisher wybierz polecenie **Zabezpieczenia** , a następnie kliknij kartę **Delegowanie** . Kliknij pole wyboru, aby włączyć "Delegate logowania i zapisów".

![Delegowanie strony][api-management-delegation-signin-up]

* Określ, co adres URL punktu końcowego usługi specjalne delegowanie zostaną i wprowadzić w polu **adres URL punktu końcowego delegowanie** . 

* W polu **klucz uwierzytelniania delegowanie** wprowadź hasła, które będą używane do obliczania podpisu, uzyskanych w celu weryfikacji upewnić się, że żądania faktycznie pochodzi zarządzania interfejsu API Azure. Kliknięcie przycisku **Generowanie** , mają Managemnet interfejsu API losowo wygenerować klucz dla Ciebie.

Następnie należy utworzyć punkt **końcowy delegowanie**. Musi wykonać wiele czynności:

1. Odbieranie żądania w następującym formacie:

    > *{adres URL strony źródłowej} http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl= i salt = {ciąg} & podpis = {ciąg}*

    Parametry zapytania w przypadku logowania / zapisów:
    - **Operacja**: określa rodzaju delegowanie żądania jest — w tym przypadku tylko może być **ekran logowania**
    - **returnUrl**: adres URL strony, gdy użytkownik kliknie łącze logowania lub zapisów
    - **soli**: ciąg soli specjalnie na potrzeby przetwarzania skrótu zabezpieczeń
    - **Podpis**: skrótu obliczoną zabezpieczeń do użycia w porównaniu do własnego obliczone mieszanie

2. Sprawdź, czy z Azure interfejsu API zarządzania pochodzi żądanie (opcjonalne, ale zdecydowanie zalecany dla zabezpieczeń)

    * Obliczenia skrótu HMAC SHA512 ciągu na podstawie parametrów kwerendy **returnUrl** i **soli** ([Kod przykład poniżej]):
        > HMAC (**soli** "\n" + **returnUrl**)
         
    * Porównanie mieszania powyżej obliczona wartość parametru zapytania **Podpis** . Jeśli są zgodne z dwóch wartości skrótów, przejdź do następnego kroku, w przeciwnym razie odrzucić żądanie.

2. Sprawdzić, czy są odbiera żądanie logowania w i znak up: parametry kwerendy **operacji** jest równa "**logowania**".

3. Prezentowanie użytkownika za pomocą interfejsu użytkownika umożliwiającego logowania lub zapisów

4. Jeśli użytkownik jest podpisywanie up musisz utworzyć odpowiednie konto dla nich w zarządzaniu interfejsu API. [Utwórz użytkownika] przy użyciu interfejsu API usługi REST zarządzania interfejsu API. Ten sposób Uważaj, Ustaw identyfikator użytkownika, do tego samego, który znajduje się w magazynie użytkownika lub Identyfikatora, który użytkownik może zachować ścieżkę.

5. Po pomyślnym uwierzytelnieniu użytkownika:

    * [żądanie tokenu (SSO) - jednokrotnej] za pośrednictwem interfejsu API usługi REST zarządzania interfejsu API

    * Dołącz returnUrl parametru zapytania do adresu URL logowania jednokrotnego otrzymane od połączenia interfejsu API powyżej:
        > https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url np. 

    * przekierować użytkownika do powyżej wyprodukowane adresu URL

Oprócz operacji **logowania** można również wykonać zarządzaniem, wykonywaniem powyższych czynności i przy użyciu jednej z następujących czynności.

-   **Element ChangePassword**
-   **ChangeProfile**
-   **CloseAccount**

Należy przekazać następujące parametry kwerendy w operacji zarządzania kontem.

-   **Operacja**: określa rodzaju żądanie delegowania jest (Element ChangePassword, ChangeProfile lub CloseAccount)
-   **Nazwa użytkownika**: identyfikator użytkownika konta do zarządzania
-   **soli**: ciąg soli specjalnie na potrzeby przetwarzania skrótu zabezpieczeń
-   **Podpis**: skrótu obliczoną zabezpieczeń do użycia w porównaniu do własnego obliczone mieszanie

## <a name="delegate-product-subscription"> </a>Delegowanie subskrypcji produktu

Delegowanie subskrypcji produktu działa podobnie do delegowania użytkownika logowania /-up. Ostateczne przepływ pracy będzie w następujący sposób:

1. Deweloper wybiera produktu w portalu zarządzania interfejsu API Deweloper i kliknie przycisk Subskrybuj
2. Przeglądarka jest przekierowywana do punktu końcowego delegowania
3. Delegowanie końcowy wykonuje kroki subskrypcji wymagane produktu — to zależy od Ciebie i może powodować przekierowywania do innej strony na żądanie informacji rozliczeniowych dotyczących dodatkowe pytania, lub po prostu przechowywanie informacji i nie wymaga żadnych działań użytkownika


Aby włączyć funkcję, na stronie **Delegowanie** kliknij **pełnomocnika produktu subskrypcji**.

Następnie upewnij się, że punkt końcowy delegowanie wykonuje następujące czynności:


1. Odbieranie żądania w następującym formacie:

    > *http://www.yourwebsite.com/apimdelegation?operation= {operacji} & productId = {produkt subskrybować} & identyfikator użytkownika = {użytkownika zgłaszającego żądanie} i soli = {ciąg} & podpis = {ciąg}*

    Parametry zapytania w przypadku subskrypcji produktu:
    - **Operacja**: Określa, jakiego rodzaju żądanie delegowania jest. Dla subskrypcji produktu żądania prawidłowe opcje są:
        - "Subskrypcji": żądanie użytkownika do danego produktu z subskrypcji udostępnionej identyfikator (patrz poniżej)
        - "Anuluj subskrypcję": żądanie, aby anulować subskrypcję użytkownika z produktem
        - "Odnawianie": requst odnowienia subskrypcji (np. może być wygasa)
    - **IDProduktu**: identyfikator produktu użytkownik zażądał subskrybowanie
    - **Nazwa użytkownika**: identyfikator użytkownika, dla którego jest wniosek
    - **soli**: ciąg soli specjalnie na potrzeby przetwarzania skrótu zabezpieczeń
    - **Podpis**: skrótu obliczoną zabezpieczeń do użycia w porównaniu do własnego obliczone mieszanie


2. Sprawdź, czy z Azure interfejsu API zarządzania pochodzi żądanie (opcjonalne, ale zdecydowanie zalecany dla zabezpieczeń)

    * Obliczanie SHA512 HMAC ciągu na podstawie parametrów kwerendy **productId**, **Nazwa użytkownika** i **soli** :
        > HMAC (**soli** "\n" + **productId** + "\n" + **Nazwa użytkownika**)
         
    * Porównanie mieszania powyżej obliczona wartość parametru zapytania **Podpis** . Jeśli są zgodne z dwóch wartości skrótów, przejdź do następnego kroku, w przeciwnym razie odrzucić żądanie.
    
3. Wykonywanie dowolnej przetwarzanie subskrypcji produktu zależy od typu operacji żądanej w **operacji** — przykład rozliczeń uzyskania odpowiedzi na pytania, itd.

4. Na pomyślne subskrypcji użytkownikowi produktu na Twojej stronie Subskrybuj użytkownikowi produktu interfejsu API zarządzania, [dzwoniąc interfejsu API usługi REST subskrypcji produktu].

## <a name="delegate-example-code"></a> Przykład kodu ##

Te przykłady kodu pokazująca, jak wykonać *klucz sprawdzania poprawności delegowanie*, który jest ustawiona na ekranie delegowanie portalu programu publisher na tworzenie HMAC, która następnie jest używana do sprawdzania poprawności podpisu, udowodnij ważności przekazano returnUrl. Ten sam kod działa w przypadku kolumn productId i identyfikator użytkownika nieznaczne modyfikacji.

**Kod C# wygenerować mieszania returnUrl**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**Kod NodeJS, aby wygenerować mieszania returnUrl**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących delegowania Zobacz poniższym klipie wideo.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[uzyskać token jedno logowania jednokrotnego (SSO)]: http://go.microsoft.com/fwlink/?LinkId=507409
[Tworzenie użytkownika]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[wywoływanie interfejsu API usługi REST subskrypcji produktu]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[Przykładowy kod poniżej]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 