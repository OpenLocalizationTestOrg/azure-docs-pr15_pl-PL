<properties 
    pageTitle="Jak używać usługi poczty e-mail SendGrid (PHP) | Microsoft Azure" 
    description="Dowiedz się, jak wysłać wiadomość e-mail z usługi poczty e-mail SendGrid Azure. Przykłady kodu napisanego w PHP." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Jak korzystać z usług poczty E-mail SendGrid PHP

Ten przewodnik pokazuje, jak wykonywać typowe zadania programowania z usługi poczty e-mail SendGrid Azure. Przykłady są zapisywane w PHP.
Scenariusze, w których zawiera **tworzenia wiadomości e-mail**, **Wysyłanie wiadomości e-mail**i **Dodawanie załączników**. Aby uzyskać więcej informacji na SendGrid i wysłać wiadomość e-mail zobacz sekcję [Następne kroki](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Co to jest usługa poczty E-mail SendGrid?

SendGrid to [Usługa poczty e-mail opartej na chmurze] , która zapewnia niezawodne [dostarczanie transakcji poczty e-mail], skalowalność i w czasie rzeczywistym analizy wraz z elastyczne interfejsów API ułatwienia integracji niestandardowych. Typowe scenariusze zastosowania SendGrid obejmują:

-   Automatycznego wysyłania potwierdzenia przeczytania dla klientów
-   Administrowanie dystrybucji listy do wysyłania klientów miesięczny ulotki e i oferty specjalne
-   Zbieranie metryki w czasie rzeczywistym dla elementów takich jak zablokowanych wiadomości e-mail i czas reakcji klienta
-   Generowanie raportów w celu identyfikowania trendów
-   Przekazywanie zapytania odbiorców
- Powiadomienia e-mail z poziomu aplikacji

Aby uzyskać więcej informacji zobacz [https://sendgrid.com][].

## <a name="create-a-sendgrid-account"></a>Tworzenie konta SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Za pomocą SendGrid z poziomu aplikacji PHP

W aplikacji Azure PHP za pomocą SendGrid wymaga żadnych specjalnych konfiguracji lub kodowania. Ponieważ SendGrid to usługa, gdzie są dostępne w taki sam sposób, z aplikacji chmury jak go z aplikacji lokalnej.

## <a name="how-to-send-an-email"></a>Jak: wysyłanie wiadomości E-mail

Możesz wysłać wiadomości e-mail za pomocą SMTP lub interfejs API sieci Web firmy SendGrid.

### <a name="smtp-api"></a>INTERFEJS API SMTP

Aby wysłać wiadomości e-mail za pomocą interfejsu API SMTP SendGrid, użyj *Efektywny Swift*, opartego na składnikach biblioteki do wysyłania wiadomości e-mail z aplikacji PHP. *Efektywny Swift* biblioteki można pobrać z v5.3.0 [http://swiftmailer.org/download][] (używaj [Kompozytor] zainstalować efektywny Swift). Wysyłanie wiadomości e-mail z biblioteką wymaga utworzenia wystąpienia <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_efektywny</span>, i <span class="auto-style2">Swift\_wiadomości</span> klasy, ustawiając odpowiednie właściwości i nawiązywania połączeń z <span class="auto-style2">Swift\_Mailer::send</span> metody.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Interfejsu API sieci Web

Za pomocą funkcji i PHP [zawinięcie funkcja][] wysyłanie wiadomości e-mail za pomocą interfejsu API sieci Web SendGrid.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

Interfejs API sieci Web firmy SendGrid jest bardzo podobne do interfejsu API usługi REST, że jest on naprawdę RESTful interfejsu API, że w większości połączeń, oba uzyskiwanie i zleceń WPIS, które mogą być używane zamiennie.

## <a name="how-to-add-an-attachment"></a>Jak: Dodawanie załącznika

### <a name="smtp-api"></a>INTERFEJS API SMTP

Wysyłanie załącznika za pomocą interfejsu API SMTP wymaga jeden dodatkowy wiersz kod przykładowy skrypt do wysyłania wiadomości e-mail z efektywny Swift.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

Dodatkowe wiersza kodu, jest następująca:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Ten wiersz kodu wywołuje metodę Dołącz <span class="auto-style2">Swift\_wiadomości</span> obiektu i jest używana metoda statyczna <span class="auto-style2">fromPath</span> <span class="auto-style2">Swift\_załącznika</span> klasy gromadzenia i dołączanie pliku do wiadomości.

### <a name="web-api"></a>Interfejsu API sieci Web

Wysyłanie załącznika za pomocą interfejsu API sieci Web jest bardzo podobne do wysyłania wiadomości e-mail za pomocą interfejsu API sieci Web. Jednak pamiętać, że w następującym przykładzie, Tablica parametru musi zawierać ten element:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Przykład:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Jak: Użyj filtrów, aby umożliwić stopki, śledzenia i analizy

SendGrid zapewnia funkcje dodatkowe wiadomości e-mail za pomocą "filtry". Są ustawienia, które można dodać do wiadomości e-mail, aby włączyć funkcję określonych przykład Włącz śledzenie kliknij pozycję, usługi Google analytics, subskrypcji śledzenia i tak dalej.

Filtry można stosować do wiadomości przy użyciu właściwości filtrów. Każdy filtr jest określona przez skrótu zawierające ustawienia specyficzne dla filtru. W poniższym przykładzie umożliwia filtrowanie stopki i określa wiadomości SMS, które zostaną dołączone do dolnej części wiadomości e-mail.
W tym przykładzie użyjemy [sendgrid php biblioteki].
[Kompozytor] należy zainstalować biblioteki:
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Przykład:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy usługa poczty E-mail SendGrid, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

-   Dokumentacja SendGrid: <https://sendgrid.com/docs>
-   Biblioteka SendGrid PHP: <https://github.com/sendgrid/sendgrid-php>
-   SendGrid oferta specjalna dla klientów Azure: <https://sendgrid.com/windowsazure.html>

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów PHP](/develop/php/).


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/Download]: http://swiftmailer.org/download
  [Funkcja zwinięcie]: http://php.net/curl
  [usługi poczty e-mail opartej na chmurze]: https://sendgrid.com/email-solutions
  [dostarczanie transakcji poczty e-mail]: https://sendgrid.com/transactional-email
  [Biblioteka sendgrid php]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Kompozytor]: https://getcomposer.org/download/
