<properties
    pageTitle="Jak używać Twilio głosowe i SMS (PHP) | Microsoft Azure"
    description="Dowiedz się, jak nawiązywanie połączenia telefonicznego i wysyłanie wiadomości SMS za pomocą interfejsu API Twilio usługi Azure. Przykłady kodu napisanego w PHP."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Jak używać Twilio głosowe i możliwości SMS PHP
Ten przewodnik pokazuje, jak wykonywać typowe zadania programowania z usługą interfejsu API Twilio Azure. Scenariusze, w których obejmują nawiązywania rozmowy telefonicznej i wysyłanie wiadomości krótkie wiadomości usługi (SMS). Aby uzyskać więcej informacji na Twilio i za pomocą głosu i wiadomości SMS w aplikacji zobacz sekcję [Następne kroki](#NextSteps) .

## <a id="WhatIs"></a>Co to jest Twilio?
Twilio jest zasilania przyszłości komunikacji firmy, dzięki czemu deweloperzy do osadzania głosowych, połączeń VoIP i wiadomości do aplikacji. Ich wirtualizuj infrastruktury wszystkie potrzebne w środowisku opartej na chmurze, globalnej Uwidacznianie go za pomocą platformy komunikacji interfejsu API Twilio. Aplikacje są prosty do tworzenia i skalowalna. Cieszyć elastyczność płatności — jako-Ci Przejdź ceny i korzystać z chmury niezawodności.

**Twilio głosu** umożliwia aplikacjom nawiązywanie i odbieranie połączeń telefonicznych. **Twilio SMS** umożliwia aplikacji do wysyłania i odbierania wiadomości SMS. **Klient Twilio** umożliwia nawiązywanie połączeń VoIP z dowolnego telefonu, tabletu lub przeglądarki i obsługuje WebRTC.

## <a id="Pricing"></a>Twilio ceny i oferty specjalne

Azure klienci otrzymywać [oferta specjalna] 10 zł kredytu Twilio podczas uaktualniania konta Twilio. Ten kredytowej Twilio można stosować do dowolnego zastosowania Twilio (kredyt $10 równoważna maksymalnie 1000 wiadomości SMS wysyłaniem i odbieraniem do 1000 ruchu przychodzącego minut głosu, w zależności od lokalizacji telefonu docelowego numer i wiadomości lub połączenia). Realizowanie tę kredytową Twilio i rozpoczynanie pracy w: [ahoy.twilio.com/azure].

Twilio to usługa płatne. Istnieją żadne opłaty konfiguracji i konto zostanie zamknięte w dowolnym momencie. Więcej informacji można znaleźć w [Ceny Twilio] [twilio_pricing].

## <a id="Concepts"></a>Pojęcia
Interfejs API Twilio jest RESTful interfejs API, który zawiera głosu i SMS funkcje dla aplikacji. Biblioteki klienta są dostępne w wielu językach; Aby uzyskać listę, zobacz [Biblioteki interfejsu API Twilio] [twilio_libraries].

Aspektami interfejsu API Twilio są zleceń Twilio i Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Czasowniki Twilio
Interfejs API korzysta z Twilio zleceń; na przykład ** &lt;powiedz&gt; ** czasownika powoduje, że Twilio komputerowi dostarczenia wiadomości w trakcie rozmowy.

Poniżej przedstawiono listę Twilio. Więcej informacji na temat innych zleceń i możliwości za pośrednictwem [Twilio Markup Language dokumentacji] [http://www.twilio.com/docs/api/twiml].

* ** &lt;Wybierania numerów&gt;**: łączy rozmówcy pod inny numer telefonu.
* ** &lt;Gromadzenie&gt;**: zbiera cyfr wprowadzane na klawiaturze telefonu.
* ** &lt;Rozłączanie&gt;**: powoduje zakończenie połączenia.
* ** &lt;Odtwarzanie&gt;**: odtwarza plik audio.
* ** &lt;Wstrzymaj&gt;**: czeka w trybie cichym na określoną liczbę sekund.
* ** &lt;Rekordu&gt;**: rekordów głosu rozmówcy i zwraca adres URL pliku, który zawiera nagrania.
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
Gdy chcesz rozpocząć konto Twilio, Utwórz konto na [Spróbuj Twilio] [try_twilio]. Możesz rozpocząć przy użyciu bezpłatnego konta i późniejszego uaktualnienia Twojego konta.

Podczas tworzenia konta w usłudze konto Twilio, otrzymasz identyfikator konta i token uwierzytelniania. Oba będą potrzebne do nawiązywania połączeń interfejsu API Twilio. Aby zapobiec nieupoważnionemu dostępowi do swojego konta, bezpieczeństwo token uwierzytelniania. Identyfikator konta i uwierzytelniania usługi token jest wyświetlana na [stronie konta Twilio] [twilio_account], w polach etykietą **identyfikator SID konta** i **AUTH TOKEN**odpowiednio.

## <a id="create_app"></a>Tworzenie aplikacji PHP
Aplikacji PHP, która korzysta z usługi Twilio i działa w Azure nie różni się od innych aplikacji PHP, która korzysta z usługi Twilio. Podczas Twilio usług są oparte na pozostałych i mogą być wywoływane PHP na kilka sposobów, w tym artykule poświęcona sposobu używania usług Twilio z [biblioteki Twilio PHP z GitHub][twilio_php]. Aby uzyskać więcej informacji na temat korzystania z biblioteki Twilio dla PHP, zobacz [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Szczegółowe instrukcje na temat Tworzenie i wdrażanie aplikacji Twilio-PHP Azure są dostępne w [jak Twilio przy użyciu rozmowy telefonicznej w aplikacji PHP Azure][howto_phonecall_php].

## <a id="configure_app"></a>Konfigurowanie aplikacji przy użyciu bibliotek Twilio
Możesz skonfigurować aplikację, aby użyć biblioteki Twilio dla PHP na dwa sposoby:

1. Pobieranie biblioteki Twilio dla PHP z GitHub ([https://github.com/twilio/twilio-php][twilio_php]) i Dodaj katalog **usług** do aplikacji.

    -LUB-

2. Zainstaluj biblioteki Twilio dla PHP jako pakiet GRUSZ. Czy można zainstalować z następujących poleceń:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Po zainstalowaniu biblioteki Twilio dla PHP, możesz dodać instrukcję **require_once** w górnej części plików PHP, aby odwołać biblioteki:

        require_once 'Services/Twilio.php';

Aby uzyskać więcej informacji, zobacz [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Jak: nawiązywanie połączenia wychodzące
Poniżej przedstawiono sposób nawiązać połączenie wychodzące za pomocą klasy **Services_Twilio** . Ten kod zawiera również witryny dostarczonego przez Twilio zwraca odpowiedź Twilio Markup Language (TwiML). Podstawianie wartości dla numerów telefonów **od** i **do** i upewnij się, sprawdź numer telefonu **z** dla Twojego konta Twilio przed uruchomieniem kodu.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Jak wspomniano, ten kod zawiera witryny Twilio-pod warunkiem, aby zwrócić odpowiedź TwiML. Zamiast tego można jego własnej witrynie o podanie odpowiedzi TwiML; Aby uzyskać więcej informacji zobacz [jak podanie odpowiedzi TwiML z swój własny witryny sieci Web](#howto_provide_twiml_responses).


- **Uwaga**: Aby rozwiązać błędy sprawdzania poprawności certyfikatów SSL, zobacz [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Jak: wysyłanie wiadomości SMS
Poniżej przedstawiono sposób wysyłanie wiadomości SMS za pomocą klasy **Services_Twilio** . Numer **z** jest udostępniany przez Twilio w przypadku kont wersji próbnej do wysyłania wiadomości SMS. **Numer** musi zostać zweryfikowane dla Twojego konta Twilio przed uruchomieniem kodu.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Jak: Podaj TwiML odpowiedzi od własną witrynę sieci Web
Gdy aplikacja inicjuje połączenie API Twilio, Twilio będzie wysyłać żądania do adresu URL, który może zwrócić odpowiedź TwiML. W powyższym przykładzie użyto adresu URL dostarczonego przez Twilio [http://twimlets.com/message][twimlet_message_url]. (Gdy TwiML jest przeznaczona dla Twilio, możesz wyświetlić ten to w przeglądarce. Na przykład kliknij [http://twimlets.com/message] [ twimlet_message_url] Aby wyświetlić pusta `<Response>` element; inny przykład kliknij [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] Aby wyświetlić `<Response>` element, który zawiera `<Say>` element.)

Zamiast wpisywać adres URL Twilio-pod warunkiem, możesz utworzyć własną witryną, która zwraca odpowiedzi HTTP. Możesz utworzyć witryny w innym języku, który zwraca odpowiedzi XML; w tym temacie założono, że będziesz używać PHP tworzenie TwiML.

Następującą stronę PHP wyników w odpowiedzi TwiML informacją **Witaj świecie** na połączenie.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Jak widać z powyższego przykładu, odpowiedź TwiML jest po prostu dokumentu XML. Biblioteka Twilio dla PHP zawiera klasy generujące TwiML dla Ciebie. W poniższym przykładzie tworzy równoważne odpowiedzi, tak jak pokazano powyżej, ale używa **usług\_Twilio\_Twiml** klasy w bibliotece Twilio dla PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Aby uzyskać więcej informacji o TwiML, zobacz [https://www.twilio.com/docs/api/twiml][twiml_reference].

Po umieszczeniu strony PHP skonfigurowane do zapewnienia TwiML odpowiedzi, użyj adres URL strony PHP adres URL przekazany do `Services_Twilio->account->calls->create` metody. Na przykład jeśli masz aplikację sieci Web o nazwie **MyTwiML** wdrożony Azure hostowanej usługi i nazwę strony PHP jest **mytwiml.php**, adres URL mogą być przekazywane do **Services_Twilio -> konta -> połączeń -> Utwórz** jak pokazano w poniższym przykładzie:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Aby uzyskać dodatkowe informacje na temat używania Twilio w Azure z PHP, zobacz [jak utworzyć Twilio przy użyciu rozmowy telefonicznej w aplikacji PHP Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Sposoby: za pomocą usług Twilio dodatkowe
Oprócz przykładach pokazano na ilustracji Twilio oferuje oparte na sieci web interfejsów API, które można wykorzystać dodatkowe funkcje Twilio z Azure aplikacji. Aby uzyskać szczegółowe informacje, zapoznaj się z [dokumentacją Twilio API] [twilio_api_documentation].

## <a id="NextSteps"></a>Następne kroki
Teraz, gdy znasz już podstawy pracy Twilio, skorzystaj z poniższych łączy, aby dowiedzieć się więcej:

* [Wskazówki dotyczące zabezpieczeń Twilio] [twilio_security_guidelines]
* [Prowadnice Twilio instrukcje i przykładowy kod] [twilio_howtos]
* [Samouczki Twilio Szybki Start][twilio_quickstarts]
* [Twilio na GitHub] [twilio_on_github]
* [Zwróć się do pomocy technicznej Twilio] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

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
