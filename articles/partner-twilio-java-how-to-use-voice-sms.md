<properties 
    pageTitle="Jak używać Twilio głosowe i SMS (Java) | Microsoft Azure" 
    description="Dowiedz się, jak nawiązywanie połączenia telefonicznego i wysyłanie wiadomości SMS za pomocą interfejsu API Twilio usługi Azure. Przykłady kodu napisany w języku Java." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Jak używać Twilio głosowe i możliwości SMS Java

Ten przewodnik pokazuje, jak wykonywać typowe zadania programowania z usługą interfejsu API Twilio Azure. Scenariusze, w których obejmują nawiązywania rozmowy telefonicznej i wysyłanie wiadomości krótkie wiadomości usługi (SMS). Aby uzyskać więcej informacji na Twilio i za pomocą głosu i wiadomości SMS w aplikacji zobacz sekcję [Następne kroki](#NextSteps) .

## <a id="WhatIs"></a>Co to jest Twilio?
Twilio to interfejs API usługi sieci web telefonii, który umożliwia tworzenie połączenia głosowe i SMS aplikacji przy użyciu istniejącego języków sieci web i umiejętności. Twilio to usługa innej firmy (nie Azure funkcji i nie produktu firmy Microsoft).

**Twilio głosu** umożliwia aplikacjom nawiązywanie i odbieranie połączeń telefonicznych. **Twilio SMS** umożliwia aplikacjom nawiązywanie i odbieranie wiadomości SMS. **Klient Twilio** umożliwia aplikacjom Włączanie komunikacji głosowej przy użyciu istniejącego połączenia z Internetem, łącznie z połączeniami urządzeń przenośnych.

## <a id="Pricing"></a>Twilio ceny i oferty specjalne
Informacje o cenach Twilio jest dostępna po [Twilio] [twilio_pricing]. Azure klienci otrzymywać [Oferta specjalna][special_offer]: bezpłatne kredytowej tekstów 1000 lub 1000 ruchu przychodzącego minut. Aby utworzyć konto w ramach tej oferty lub uzyskać więcej informacji, odwiedź stronę [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Pojęcia
Interfejs API Twilio jest RESTful interfejs API, który zawiera głosu i SMS funkcje dla aplikacji. Biblioteki klienta są dostępne w wielu językach; Aby uzyskać listę, zobacz [Biblioteki interfejsu API Twilio] [twilio_libraries].

Aspektami interfejsu API Twilio są zleceń Twilio i Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Czasowniki Twilio
Interfejs API korzysta z Twilio zleceń; na przykład ** &lt;powiedz&gt; ** czasownika powoduje, że Twilio komputerowi dostarczenia wiadomości w trakcie rozmowy. 

Poniżej przedstawiono listę Twilio.

* ** &lt;Wybierania numerów&gt;**: łączy rozmówcy pod inny numer telefonu.
* ** &lt;Gromadzenie&gt;**: zbiera cyfr wprowadzane na klawiaturze telefonu.
* ** &lt;Rozłączanie&gt;**: powoduje zakończenie połączenia.
* ** &lt;Odtwarzanie&gt;**: odtwarza plik audio.
* ** &lt;Wstrzymaj&gt;**: czeka w trybie cichym na określoną liczbę sekund.
* ** &lt;Rekord&gt;**: rekordów rozmówcy głosu i zwraca adres URL pliku, który zawiera nagrania.
* ** &lt;Przekierowywać&gt;**: przekazuje sterowanie połączenia lub wiadomości SMS do TwiML u innego adresu URL.
* ** &lt;Odrzuć&gt;**: odrzuci połączenia przychodzącego swój numer Twilio bez możesz rozliczenia
* ** &lt;Powiedz&gt;**: Konwertuje tekst na mowę, wykonany w trakcie rozmowy.
* ** &lt;Sms&gt;**: wysyła wiadomość SMS.

### <a id="TwiML"></a>TwiML
TwiML jest zestawem instrukcji opartym na języku XML według zleceń Twilio, które pomagają w Twilio sposobu przetwarzania połączenia lub wiadomości SMS.

Na przykład następujące TwiML chcesz przekonwertować tekst **Witaj świecie** mowa.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Gdy aplikacja wywołuje interfejs API Twilio, jeden z parametrów interfejsu API jest adres URL, który zwraca odpowiedź TwiML. Na potrzeby rozwoju dostarczonego przez Twilio adresy URL umożliwia podanie odpowiedzi TwiML używanych przez aplikacje. Może również udostępnić własne adresy URL do utworzenia odpowiedzi TwiML, a innym rozwiązaniem jest użycie obiektu **TwiMLResponse** .

Aby uzyskać więcej informacji o Twilio zleceń, atrybuty i TwiML, zobacz [TwiML] [twiml]. Aby uzyskać dodatkowe informacje na temat interfejsu API Twilio, zobacz [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Tworzenie konta Twilio
Gdy wszystko będzie już gotowe do Uzyskiwanie konta Twilio, Utwórz konto u [Spróbuj Twilio] [try_twilio]. Możesz rozpocząć z bezpłatnego konta i późniejszego uaktualnienia konta.

Podczas tworzenia konta dla konta Twilio, otrzymasz identyfikator konta i token uwierzytelniania. Oba będą potrzebne do nawiązywania połączeń interfejsu API Twilio. Aby zapobiec nieupoważnionemu dostępowi do swojego konta, bezpieczeństwo token uwierzytelniania. Identyfikator konta i uwierzytelniania usługi token jest wyświetlana na [stronie konta Twilio] [twilio_account], w polach etykietą **identyfikator SID konta** i **AUTH TOKEN**odpowiednio.

## <a id="create_app"></a>Tworzenie aplikacji Java
1. Uzyskiwanie Twilio JAR i dodaj go ścieżce kompilacji Java i zestawu wdrożenia usługi też. W [https://github.com/twilio/twilio-java][twilio_java], można pobrać GitHub źródeł i tworzyć własne SŁOIK lub pobieranie gotowych SŁOIK (z lub bez zależności).
2. Upewnij się, elementu z JDK **cacerts** keystore zawiera certyfikat urząd certyfikacji bezpiecznego firmy Equifax z MD5 odcisku palca 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (numer seryjny jest 35:DE:F4:CF i odcisku palca SHA1 jest D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Jest to urząd certyfikacji certyfikat dla [https://api.twilio.com] [ twilio_api_service] usługa, która jest wywoływana, gdy używasz interfejsy API Twilio. W przypadku informacji na temat sprawdzania usługi JDK **cacerts** elementu keystore zawiera poprawny certyfikat urzędu certyfikacji, zobacz temat [Dodawanie certyfikatu w magazynie certyfikatów urzędu certyfikacji Java][add_ca_cert].

Szczegółowe instrukcje dotyczące korzystania z biblioteki klienta Twilio dla języka Java są dostępne w [jak Twilio przy użyciu rozmowy telefonicznej w aplikacji Java Azure][howto_phonecall_java].

## <a id="configure_app"></a>Konfigurowanie aplikacji przy użyciu bibliotek Twilio
W kodzie możesz dodać **Importowanie** instrukcji w górnej części plików źródłowych dla pakietów Twilio lub klasy, który ma być wyświetlany w aplikacji. 

Dla plików źródłowych Java:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Pliki źródłowe strony serwera Java (JSP):

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
W zależności od tego, które pakiety Twilio lub klasy chcesz użyć oficjalnych dokumentów **Importowanie** mogą się różnić.

## <a id="howto_make_call"></a>Jak: nawiązywanie połączenia wychodzące
Poniżej przedstawiono sposób nawiązać połączenie wychodzące za pomocą klasy **CallFactory** . Ten kod zawiera również witryny dostarczonego przez Twilio zwraca odpowiedź Twilio Markup Language (TwiML). Podstawianie wartości dla numerów telefonów **od** i **do** i upewnij się, sprawdź numer telefonu **z** dla Twojego konta Twilio przed uruchomieniem kodu.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

Aby uzyskać więcej informacji na temat parametrów przekazany do metody **CallFactory.create** , zobacz [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Jak wspomniano, ten kod zawiera witryny Twilio-pod warunkiem, aby zwrócić odpowiedź TwiML. Zamiast tego można jego własnej witrynie o podanie odpowiedzi TwiML; Aby uzyskać więcej informacji zobacz [jak podanie TwiML odpowiedzi w aplikacji języka Java Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Jak: wysyłanie wiadomości SMS
Poniżej przedstawiono sposób wysyłanie wiadomości SMS za pomocą klasy **SmsFactory** . **Z** numer, **4155992671**jest udostępniany przez Twilio w przypadku kont wersji próbnej do wysyłania wiadomości SMS. **Numer** musi zostać zweryfikowane dla Twojego konta Twilio przed uruchomieniem kodu.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
Aby uzyskać więcej informacji na temat parametrów przekazany do metody **SmsFactory.create** , zobacz [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Jak: Podaj TwiML odpowiedzi od własną witrynę sieci Web
Gdy aplikacja inicjuje połączenie API Twilio, na przykład przy użyciu metody **CallFactory.create** Twilio będzie wysyłać żądania do adresu URL, który może zwrócić odpowiedź TwiML. W powyższym przykładzie użyto adresu URL dostarczonego przez Twilio [http://twimlets.com/message][twimlet_message_url]. (Gdy TwiML jest przeznaczona dla usług sieci Web, możesz wyświetlić TwiML w przeglądarce. Na przykład kliknij [http://twimlets.com/message] [ twimlet_message_url] Aby wyświetlić pusta ** &lt;odpowiedź&gt; ** element; inny przykład kliknij [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] Aby wyświetlić ** &lt;odpowiedź&gt; ** element, który zawiera ** &lt;powiedz&gt; ** element.)

Zamiast wpisywać adres URL Twilio-pod warunkiem, możesz utworzyć własną witryną adresu URL, która zwraca odpowiedzi HTTP. Możesz utworzyć witryny w innym języku, która zwraca odpowiedzi HTTP; w tym temacie założono, że będzie hostingu adres URL strony JSP.

Następującą stronę JSP wyników w odpowiedzi TwiML informacją **Witaj świecie** na połączenie.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

Następującą stronę JSP wyników w odpowiedzi TwiML tekst z opisem, występują przerwy w działaniu kilka i opisem informacje o wersji interfejsu API Twilio i nazwa roli Azure.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

Parametr **ApiVersion** jest dostępna w Twilio głosu żądania (nie żądania usługi SMS). Aby wyświetlić parametry dostępne żądania Twilio głosu i żądań wiadomości SMS, zobacz temat <https://www.twilio.com/docs/api/twiml/twilio_request> i <https://www.twilio.com/docs/api/twiml/sms/twilio_request>. Zmienna środowiska **RoleName** jest dostępna w ramach wdrożenia usługi Azure. (Jeśli chcesz dodać zmienne środowiska niestandardowe, aby ta osoba może zostać pobrana z **System.getenv**, zobacz sekcję zmienne środowiska na [Różne ustawienia konfiguracji roli][misc_role_config_settings].)

Po umieszczeniu strony JSP skonfigurowane do zapewnienia TwiML odpowiedzi, użyj adres URL strony JSP jako adres URL przekazany do metody **CallFactory.create** . Na przykład jeśli masz aplikację sieci Web o nazwie MyTwiML wdrożony Azure hostowanej usługi i nazwę strony JSP jest mytwiml.jsp, adres URL mogą być przekazywane do **CallFactory.create** , jak pokazano poniżej:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Innym rozwiązaniem dla odpowiadać TwiML jest za pośrednictwem klasy **TwiMLResponse** , który jest dostępny w pakiecie **com.twilio.sdk.verbs** .

Aby uzyskać dodatkowe informacje o korzystaniu z platformy Azure przy użyciu języka Java Twilio, zobacz [jak utworzyć Twilio przy użyciu rozmowy telefonicznej w aplikacji Java Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Sposoby: za pomocą usług Twilio dodatkowe
Oprócz przykładach pokazano na ilustracji Twilio oferuje oparte na sieci web interfejsów API, które można wykorzystać dodatkowe funkcje Twilio z Azure aplikacji. Aby uzyskać szczegółowe informacje, zapoznaj się z [dokumentacją Twilio API] [twilio_api_documentation].

## <a id="NextSteps"></a>Następne kroki
Teraz, gdy znasz już podstawy pracy Twilio, skorzystaj z poniższych łączy, aby dowiedzieć się więcej:

* [Wskazówki dotyczące zabezpieczeń Twilio] [twilio_security_guidelines]
* [Instrukcje Twilio i przykładowy kod] [twilio_howtos]
* [Samouczki Twilio Szybki Start][twilio_quickstarts] 
* [Twilio na GitHub] [twilio_on_github]
* [Zwróć się do pomocy technicznej Twilio] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
