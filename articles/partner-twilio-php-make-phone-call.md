<properties
    pageTitle="Jak skonfigurować połączenie telefoniczne z Twilio (PHP) | Microsoft Azure"
    description="Dowiedz się, jak nawiązywanie połączenia telefonicznego i wysyłanie wiadomości SMS za pomocą interfejsu API Twilio usługi Azure. Próbki są PHP aplikacji."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Jak skonfigurować połączenie telefoniczne przy użyciu Twilio w aplikacji PHP Azure

W poniższym przykładzie pokazano, jak Twilio umożliwia nawiązywanie połączenia ze strony sieci web PHP obsługiwany w Azure. Wynikowa aplikacji wyświetli monit dla wartości rozmowy telefonicznej, jak pokazano na poniższym obrazie zrzut.

![Formularz Azure połączenia przy użyciu Twilio i PHP][twilio_php]

Konieczne będzie wykonaj poniższe czynności, aby użyć kodu w tym temacie:

1. Uzyskiwanie konta Twilio i uwierzytelniania token. Aby rozpocząć pracę z Twilio, oszacować ceny na [http://www.twilio.com/pricing][twilio_pricing]. Możesz zalogować konto wersji próbnej u [https://www.twilio.com/try-twilio][try_twilio]. Uzyskać informacji na temat interfejsu API dostarczony przez Twilio, zobacz [http://www.twilio.com/api][twilio_api].
2. Pobierz [Twilio biblioteki PHP](https://github.com/twilio/twilio-php) lub zainstalować go jako pakiet GRUSZ. Aby uzyskać więcej informacji zobacz [plik readme](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Zainstaluj Azure SDK dla PHP. Zawiera omówienie SDK i instrukcje dotyczące instalacji, zobacz [Konfigurowanie SDK Azure dla PHP][setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Tworzenie formularza sieci web nawiązywania połączenia

Poniższy kod HTML przedstawiono sposób tworzenia strony sieci web (**callform.html**), która pobiera dane użytkownika do nawiązywania połączenia:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Utwórz kod, aby nawiązać połączenie
Poniższy kod pokazano, jak tworzyć strony sieci web (**makecall.php**), nazywany po przesłaniu formularza przez **callform.html**wyświetlany przez użytkownika. Kod, jak pokazano poniżej tworzy wiadomość połączenia i generuje połączenie. (Używaj swojego konta Twilio i uwierzytelniania token zamiast wartości symbol zastępczy przypisane do **$sid** i **$token** w kodzie poniżej).

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Oprócz nawiązywania połączenia, **makecall.php** Wyświetla niektóre metadane połączenia (Przykłady pokazano poniżej zrzut ekranu). Aby uzyskać więcej informacji o metadanych połączenia, zobacz [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Odpowiedź Azure połączenia przy użyciu Twilio i PHP][twilio_php_response]

## <a name="run-the-application"></a>Uruchamianie aplikacji
Następnym krokiem jest wdrażania aplikacji do witryn sieci Web Azure. Poniższe artykuły zawierają informacje dotyczące tworzenia witryny sieci Web i wdrażanie kodu przy użyciu cyfra, FTP lub WebMatrix (ale nie wszystkie informacje w każdym artykule ma znaczenie):

* [Tworzenie witryny sieci Web Azure PHP MySQL i wdrażanie przy użyciu cyfra][website-git]
* [Tworzenie witryny sieci Azure Web PHP MySQL i wdrażanie przy użyciu FTP][website-ftp]

## <a name="next-steps"></a>Następne kroki
Kod podano Tobie podstawowe funkcje przy użyciu Twilio w PHP Azure. Przed wdrożeniem Azure produkcji, można dodać więcej obsługi błędów i innych elementów. Na przykład:

* Zamiast używać formularza sieci web, można Azure magazyn obiektów blob lub bazy danych SQL do przechowywania numerów telefonów i połączeń tekstu. Aby dowiedzieć się, jak przy użyciu obiektów blob platformy Azure miejsca do magazynowania w PHP, zobacz [Przy użyciu magazyn Azure z aplikacjami PHP][howto_blob_storage_php]. Aby uzyskać informacji na temat używania bazy danych SQL w PHP, zobacz [Za pomocą bazy danych SQL z aplikacjami PHP][howto_sql_azure_php].
* W kodzie **makecall.php** za pomocą adresu URL dostarczonego przez Twilio ([http://twimlets.com/message][twimlet_message_url]) o podanie odpowiedź Twilio Markup Language (TwiML), która zostanie wyświetlona informacja Twilio, jak przystąpić do połączenia. Na przykład może zawierać TwiML, który został zwrócony `<Say>` czasownika powstały w tekście wymowę odbiorcy. Zamiast używać dostarczonego przez Twilio adres URL, można utworzyć usługi na odpowiedź na żądanie w Twilio; Aby uzyskać więcej informacji, zobacz [jak Twilio Użyj głosowe i możliwości SMS PHP][howto_twilio_voice_sms_php]. Więcej informacji na temat TwiML można znaleźć w [http://www.twilio.com/docs/api/twiml][twiml]oraz więcej informacji o `<Say>` i inne zlecenia Twilio można znaleźć w [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Przeczytaj wskazówki dotyczące zabezpieczeń Twilio w [https://www.twilio.com/docs/security][twilio_docs_security].

Aby uzyskać dodatkowe informacje o Twilio, zobacz [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Zobacz też
* [Jak używać Twilio głosowe i możliwości SMS PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
