<properties 
    pageTitle="Jak używać Twilio głosowe i SMS (dopiskiem) | Microsoft Azure" 
    description="Dowiedz się, jak nawiązywanie połączenia telefonicznego i wysyłanie wiadomości SMS za pomocą interfejsu API Twilio usługi Azure. Przykłady kodu napisanego w dopiskiem." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Jak używać Twilio głosowe i możliwości SMS dopiskiem
Ten przewodnik pokazuje, jak wykonywać typowe zadania programowania z usługą interfejsu API Twilio Azure. Scenariusze, w których obejmują nawiązywania rozmowy telefonicznej i wysyłanie wiadomości krótkie wiadomości usługi (SMS). Aby uzyskać więcej informacji na Twilio i za pomocą głosu i wiadomości SMS w aplikacji zobacz sekcję [Następne kroki](#NextSteps) .

## <a id="WhatIs"></a>Co to jest Twilio?
Twilio to interfejs API usługi sieci web telefonii, który umożliwia tworzenie połączenia głosowe i SMS aplikacji przy użyciu istniejącego języków sieci web i umiejętności. Twilio to usługa innej firmy (nie Azure funkcji i nie produktu firmy Microsoft).

**Twilio głosu** umożliwia aplikacjom nawiązywanie i odbieranie połączeń telefonicznych. **Twilio SMS** umożliwia aplikacjom nawiązywanie i odbieranie wiadomości SMS. **Klient Twilio** umożliwia aplikacjom Włączanie komunikacji głosowej przy użyciu istniejącego połączenia z Internetem, łącznie z połączeniami urządzeń przenośnych.

## <a id="Pricing"></a>Twilio ceny i oferty specjalne
Informacje o cenach Twilio jest dostępna po [Twilio] [twilio_pricing]. Azure klienci otrzymywać [Oferta specjalna][special_offer]: bezpłatne kredytowej tekstów 1000 lub 1000 ruchu przychodzącego minut. Aby utworzyć konto w ramach tej oferty lub uzyskać więcej informacji, odwiedź stronę [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Pojęcia
Interfejs API Twilio jest RESTful interfejs API, który zawiera głosu i SMS funkcje dla aplikacji. Biblioteki klienta są dostępne w wielu językach; Aby uzyskać listę, zobacz [Biblioteki interfejsu API Twilio] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML jest zestawem instrukcji opartym na języku XML, które pomagają w Twilio sposobu przetwarzania połączenia lub wiadomości SMS.

Na przykład następujące TwiML chcesz przekonwertować tekst **Witaj świecie** mowa.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Wszystkich dokumentów TwiML `<Response>` jako ich elementu głównego. W tym miejscu umożliwia zleceń Twilio definiują zachowanie aplikacji.

### <a id="Verbs"></a>TwiML zleceń
Czasowniki Twilio są znaczniki XML, informujących Twilio co **robić**. Na przykład ** &lt;powiedz&gt; ** czasownika powoduje, że Twilio komputerowi dostarczenia wiadomości w trakcie rozmowy. 

Poniżej przedstawiono listę Twilio.

* ** &lt;Wybierania numerów&gt;**: łączy rozmówcy pod inny numer telefonu.
* ** &lt;Gromadzenie&gt;**: zbiera cyfr wprowadzane na klawiaturze telefonu.
* ** &lt;Rozłączanie&gt;**: powoduje zakończenie połączenia.
* ** &lt;Odtwarzanie&gt;**: odtwarza plik audio.
* ** &lt;Wstrzymaj&gt;**: czeka w trybie cichym na określoną liczbę sekund.
* ** &lt;Rekordu&gt;**: rekordów rozmówcy głosu i zwraca adres URL pliku, który zawiera nagrania.
* ** &lt;Przekierowywać&gt;**: przekazuje sterowanie połączenia lub wiadomości SMS do TwiML u innego adresu URL.
* ** &lt;Odrzuć&gt;**: odrzuci połączenia przychodzącego swój numer Twilio bez możesz rozliczenia
* ** &lt;Powiedz&gt;**: Konwertuje tekst na mowę, wykonany w trakcie rozmowy.
* ** &lt;Sms&gt;**: wysyła wiadomość SMS.

Aby uzyskać więcej informacji o Twilio zleceń, atrybuty i TwiML, zobacz [TwiML] [twiml]. Aby uzyskać dodatkowe informacje na temat interfejsu API Twilio, zobacz [Twilio API] [twilio_api].

## <a id="CreateAccount"></a>Tworzenie konta Twilio
Gdy wszystko będzie już gotowe do Uzyskiwanie konta Twilio, Utwórz konto u [Spróbuj Twilio] [try_twilio]. Możesz rozpocząć przy użyciu bezpłatnego konta i późniejszego uaktualnienia Twojego konta.

Podczas tworzenia konta dla konta Twilio, zostanie wyświetlony numer telefonu bezpłatnej aplikacji. Otrzymasz również konta SID i token uwierzytelniania. Oba będą potrzebne do nawiązywania połączeń interfejsu API Twilio. Aby zapobiec nieupoważnionemu dostępowi do swojego konta, bezpieczeństwo token uwierzytelniania. Swoje konto SID i auth token jest wyświetlana na [stronie konta Twilio][twilio_account], w polach etykietą **identyfikator SID konta** i **AUTH TOKEN**odpowiednio.

### <a id="VerifyPhoneNumbers"></a>Sprawdź numery telefonów
Oprócz numer podany przez Twilio, można też sprawdzić liczby że użytkownik może kontrolować (to znaczy telefon komórkowy lub domowy numer telefonu) do użycia w aplikacjach. 

Aby uzyskać informacje na temat zweryfikować numer telefonu, zobacz [Zarządzanie numerów] [verify_phone].

## <a id="create_app"></a>Tworzenie aplikacji dopiskiem fonetycznym
Dopiskiem fonetycznym aplikacji, która korzysta z usługi Twilio i działa w Azure nie różni się od dowolnej dopiskiem fonetycznym aplikacji, która korzysta z usługi Twilio. Podczas Twilio usługi są RESTful i może zostać wywołana z dopiskiem na kilka sposobów, w tym artykule poświęcona sposobu używania usług Twilio z [dopiskiem w bibliotece pomocy Twilio][twilio_ruby].

Przede wszystkim, [konfiguracji nowego maszyn wirtualnych Linux Azure] [ azure_vm_setup] ma pełnić rolę hosta dla nowej aplikacji sieci web dopiskiem fonetycznym. Ignoruj czynności dotyczące tworzenia aplikacji szyn, po prostu konfiguracji maszyn wirtualnych. Upewnij się, że tworzenie punktu końcowego z zewnętrznego portu 80 i wewnętrzny port 5000.

W poniższych przykładach użyjemy [Sinatra][sinatra], bardzo proste web podstawę dopiskiem. Ale na pewno umożliwiają biblioteki Pomocy Twilio dopiskiem z dowolnego innego web framework, łącznie z dopiskiem przy użyciu prowadnic.

SSH do Twojej nowej maszyn wirtualnych i Utwórz katalog dla nowej aplikacji. W tym katalogu Utwórz w pliku o nazwie Gemfile i skopiuj poniższy kod do niego:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

W wierszu polecenia Uruchom `bundle install`. Zostanie zainstalowany zależności powyżej. Następnie utworzyć plik o nazwie `web.rb`. Są to miejsce, w którym znajduje się kod dla aplikacji sieci web. Wklej następujący kod do niego:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

W tym momencie powinno być możliwe Uruchom polecenie `ruby web.rb -p 5000`. To będzie pokrętła up małych serwera na porcie 5000. Czy można przejdź do tej aplikacji w przeglądarce, odwiedź witrynę adresu URL możesz konfiguracji maszyn wirtualnych usługi Azure. Gdy można uzyskać dostęp do aplikacji sieci web w przeglądarce, możesz rozpocząć tworzenie aplikacji Twilio.

## <a id="configure_app"></a>Konfigurowanie aplikacji używać Twilio
Możesz skonfigurować aplikacji sieci web, aby użyć biblioteki Twilio aktualizując usługi `Gemfile` ma zawierać ten wiersz:

    gem 'twilio-ruby'

W wierszu polecenia Uruchom `bundle install`. Teraz otworzyć `web.rb` , w tym wierszem u góry:

    require 'twilio-ruby'

Teraz wszystko jest gotowe do używania biblioteki Pomocy Twilio dla dopiskiem w aplikacji sieci web.

## <a id="howto_make_call"></a>Jak: nawiązywanie połączenia wychodzące
Poniżej przedstawiono sposób nawiązać połączenie wychodzące. Podstawowe pojęcia obejmują za pomocą biblioteki Pomocy Twilio dopiskiem na potrzeby połączeń interfejsu API usługi REST i renderowania TwiML. Podstawianie wartości dla numerów telefonów **od** i **do** i upewnij się, sprawdź numer telefonu **z** dla Twojego konta Twilio przed uruchomieniem kodu.

Dodawanie tej funkcji do `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Jeśli otwarty wzrost `http://yourdomain.cloudapp.net/make_call` w przeglądarce, który spowoduje wywołanie interfejsu API Twilio nawiązać z kimś połączenie telefoniczne. Dwa pierwsze parametrów w `client.account.calls.create` są dość oczywiste: liczba jest `from` i liczba jest `to`. 

Trzeci parametr (`url`) jest to adres URL żądające Twilio, aby uzyskać instrukcje, co zrobić, gdy połączenie zostanie nawiązane. W tym przypadku możemy konfiguracji na adres URL (`http://yourdomain.cloudapp.net`) zwraca prosty dokument TwiML i używa `<Say>` czasownika niektórych zamiany tekstu na mowę i powiedzieć "Witaj MAŁPA" umożliwia osobie odbierającej połączenie.

## <a id="howto_recieve_sms"></a>Jak: Odbierz wiadomości SMS
W poprzednim przykładzie, możemy zainicjowane **wychodzącej** rozmowy telefonicznej. Ten czas, można użyć numeru telefonu, który Twilio przekazała us podczas zapisywania przetwarzania **przychodzących** wiadomości SMS.

Po pierwsze, zaloguj się do pulpitu [nawigacyjnego Twilio][twilio_account]. Polecenie "Numerów" w górnym obszarze nawigacyjnym, a następnie kliknij na numer Twilio, który został dostarczony. Zostaną wyświetlone dwa adresy URL, które można skonfigurować. Adres URL żądania URL żądania głosu i SMS. Są to adresy URL, które Twilio połączeń wykonany rozmowy telefonicznej lub wiadomość SMS są wysyłane do numeru. Adresy URL są także nazywane "haki sieci web".

Chcemy procesu przychodzących wiadomości SMS, więc warto zaktualizować adres URL umożliwiający `http://yourdomain.cloudapp.net/sms_url`. Teraz, a następnie kliknij przycisk Zapisz zmiany u dołu strony. Teraz ponownie `web.rb` Przejdźmy program naszych aplikacji do obsługi to:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Po wprowadzeniu zmian, upewnij się ponownie uruchomić aplikacji sieci web. Teraz wykupić telefonu i Wyślij wiadomość SMS do numeru Twilio. Należy szybko uzyskać odpowiedzi SMS informacją, że "Hey, dziękuję ping! Skale Twilio i Azure! ".

## <a id="additional_services"></a>Sposoby: za pomocą usług Twilio dodatkowe
Oprócz przykładach pokazano na ilustracji Twilio oferuje oparte na sieci web interfejsów API, które można wykorzystać dodatkowe funkcje Twilio z Azure aplikacji. Aby uzyskać szczegółowe informacje, zapoznaj się z [dokumentacją Twilio API] [twilio_api_documentation].

### <a id="NextSteps"></a>Następne kroki
Teraz, gdy znasz już podstawy pracy Twilio, skorzystaj z poniższych łączy, aby dowiedzieć się więcej:

* [Wskazówki dotyczące zabezpieczeń Twilio] [twilio_security_guidelines]
* [Twilio HowTos i przykładowy kod] [twilio_howtos]
* [Samouczki Twilio Szybki Start][twilio_quickstarts] 
* [Twilio na GitHub] [twilio_on_github]
* [Zwróć się do pomocy technicznej Twilio] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





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
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
