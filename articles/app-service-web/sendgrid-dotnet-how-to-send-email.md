<properties 
    pageTitle="Jak używać usługi poczty e-mail SendGrid (.NET) | Microsoft Azure" 
    description="Dowiedz się, jak wysłać wiadomość e-mail z usługi poczty e-mail SendGrid Azure. Kodu próbki napisana C# i za pomocą interfejsu API .NET." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Jak wysłać wiadomości E-mail za pomocą SendGrid z platformy Azure


## <a name="overview"></a>Omówienie

Ten przewodnik pokazuje, jak wykonywać typowe zadania programowania z usługi poczty e-mail SendGrid Azure. Przykłady są zapisywane w C\#
i korzystać z interfejsu API .NET. Scenariusze, w których zawierać **tworzenia wiadomości e-mail**, **Wysyłanie wiadomości e-mail**, **Dodawanie załączników**i **przy użyciu filtrów**. Aby uzyskać więcej informacji na SendGrid i wysłać wiadomość e-mail zobacz sekcję [Następne kroki][] .

## <a name="what-is-the-sendgrid-email-service"></a>Co to jest usługa poczty e-mail SendGrid?

SendGrid to [Usługa poczty e-mail opartej na chmurze] , która zapewnia niezawodne [dostarczanie transakcji poczty e-mail], skalowalność i analizy w czasie rzeczywistym wraz z elastyczne interfejsów API ułatwienia integracji niestandardowych. Typowe scenariusze zastosowania SendGrid obejmują:

-   Automatycznego wysyłania potwierdzenia do klientów.
-   Administrowanie dystrybucji list wysyłania klienci miesięczny ulotki e i oferty specjalne.
-   Zbieranie metryki w czasie rzeczywistym dla elementów takich jak zablokowanych wiadomości e-mail i czas reakcji klienta.
-   Generowanie raportów w celu identyfikowania trendów.
-   Przekazywanie zapytania klientów.
-   Przetwarzanie przychodzących wiadomości e-mail.

Aby uzyskać więcej informacji zobacz [https://sendgrid.com](https://sendgrid.com) lub nasze [C# biblioteki][języka csharp sendgrid]

## <a name="create-a-sendgrid-account"></a>Tworzenie konta SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Biblioteka klas SendGrid .NET odwołania

[Pakiet SendGrid NuGet](https://www.nuget.org/packages/Sendgrid) jest najprostszym sposobem, aby uzyskać interfejsu API SendGrid i konfigurowanie aplikacji z wszystkie zależności. NuGet to rozszerzenie programu Visual Studio dostępnych w programie Microsoft Visual Studio 2015 ułatwiający instalowanie i aktualizowanie narzędzia i bibliotek. 

> [AZURE.NOTE] Aby zainstalować NuGet, jeśli korzystasz z programu Visual Studio w wersji wcześniejszej niż Visual Studio 2015, odwiedź stronę [http://www.nuget.org](http://www.nuget.org), a następnie kliknij przycisk **Zainstaluj NuGet** .

Aby zainstalować pakiet SendGrid NuGet w aplikacji, wykonaj następujące czynności:

1.  Utworzyć nowy projekt.

    ![Tworzenie nowego projektu][create-new-project]

2.  Wybierz szablon.

    ![Wybieranie szablonu][select-a-template]

3.  W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy **odwołania**, a następnie kliknij pozycję **Zarządzaj pakiety NuGet**.

4.  Wyszukaj **SendGrid** i wybierz element **SendGrid** na liście wyników.

    ![Pakiet SendGrid NuGet][SendGrid-NuGet-package]

5.  Kliknij przycisk **Zainstaluj** , aby ukończyć instalację, a następnie zamknij to okno dialogowe.

Biblioteka klas .NET SendGrid i nosi nazwę **SendGridMail**. Zawiera następujące obszary nazw:

-   **SendGridMail** tworzenia i pracy z elementami poczty e-mail.
-   **SendGridMail.Transport** do wysyłania wiadomości e-mail za pomocą protokołu **SMTP** lub protokołu HTTP 1.1 z **Sieci Web i pozostałe**.

Dodaj następujące deklaracje nazw kodu do górnej części dowolnej C\# pliku, w którym chcesz programowy dostęp do usługi poczty e-mail SendGrid.
**System.Net** i **System.Net.Mail** są nazw .NET Framework, które są dołączone, ponieważ zawierają typów, które będą najczęściej z interfejsów API SendGrid.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Jak: Tworzenie wiadomości e-mail

Tworzenie wiadomości e-mail za pomocą obiektu **SendGridMessage** . Po utworzeniu obiektu wiadomości, można ustawić właściwości i metod, w tym nadawcy wiadomości e-mail, adresat poczty e-mail i temat oraz treść wiadomości e-mail.

Poniższy przykład przedstawia sposób tworzenia obiektu w pełni wypełnione poczty e-mail:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Aby uzyskać więcej informacji na wszystkie właściwości i metody obsługiwane przez typ **SendGrid** zobacz [języka csharp sendgrid][] na GitHub.

## <a name="how-to-send-an-email"></a>Jak: wysyłanie wiadomości e-mail

Po utworzeniu wiadomości e-mail, możesz wysłać go za pomocą interfejsu API sieci Web firmy SendGrid. Możesz także użyj [. W sieci wbudowany w bibliotece](https://sendgrid.com/docs/Code_Examples/csharp.html).

Wysyłanie wiadomości e-mail wymaga podania poświadczenia konta SendGrid (nazwa użytkownika i hasło) lub klucz interfejsu API SendGrid. Klucz interfejsu API jest preferowana metoda. Jeśli potrzebujesz, aby uzyskać szczegółowe informacje dotyczące konfigurowania interfejsu API klawiszy, odwiedź naszą [dokumentację](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Możesz zachować tych poświadczeń za pośrednictwem usługi Portal Azure, klikając pozycję Konfiguruj i dodawanie par klucz wartość w obszarze "ustawień aplikacji".

 ![Ustawienia aplikacji Azure][azure_app_settings]

 Następnie możesz może uzyskać do nich dostęp w następujący sposób: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Przy użyciu poświadczeń:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

Za pomocą klucz interfejsu API:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


W poniższych przykładach pokazano, jak wysyłać wiadomości przy użyciu interfejsu API sieci Web.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Jak: Dodawanie załącznika

Wywoływanie metody **AddAttachment** i określając nazwę i ścieżkę pliku, który chcesz dołączyć do wiadomości można dodawać załączników.
Wiele załączników można dołączyć, dzwoniąc tej metody, gdy dla każdego pliku chcesz dołączyć. W poniższym przykładzie pokazano, dodawanie załącznika do wiadomości:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Załączniki można również dodać z danych **strumienia**. Można przeprowadzić, dzwoniąc do tej samej metody co powyżej **AddAttachment**, ale przy przekazanie strumienia danych i nazwę pliku chcesz go w celu wyświetlenia w wiadomości. W tym przypadku należy dodać bibliotekę System.IO.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Jak: Włączanie stopki, śledzenia i analizy za pomocą aplikacji

SendGrid zapewnia funkcje dodatkowe wiadomości e-mail za pomocą aplikacji. Ustawienia, które można dodać do wiadomości e-mail, aby włączyć określonych funkcji, takich jak śledzenie kliknij pozycję usługi Google analytics, subskrypcji śledzenie i tak dalej. Aby uzyskać pełną listę aplikacji zobacz [Ustawienia aplikacji][].

Aplikacje można stosować do **SendGrid** wiadomości e-mail przy użyciu metody realizowane w ramach klasy **SendGrid** .

Poniższych przykładach pokazano, stopki i kliknij pozycję śledzenie filtry:

### <a name="footer"></a>Stopki

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Kliknij przycisk Śledzenie

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Sposoby: za pomocą dodatkowych usług SendGrid

SendGrid oferuje interfejsów API i webhooks, w którym można korzystać z dodatkowych funkcji SendGrid z Azure aplikacji sieci web. Aby uzyskać szczegółowe informacje zapoznaj się z [dokumentacją SendGrid API][].

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy usługa poczty E-mail SendGrid, skorzystaj z poniższych łączy, aby dowiedzieć się więcej.

*   SendGrid C\# repo biblioteki: [języka csharp sendgrid][]
*   Dokumentacja SendGrid API: <https://sendgrid.com/docs>
*   SendGrid oferta specjalna dla klientów Azure: [https://sendgrid.com](https://sendgrid.com)

  [Następne kroki]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [języka csharp sendgrid]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [Ustawienia aplikacji]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [Dokumentacja SendGrid API]: https://sendgrid.com/docs
  
  [usługi poczty e-mail opartej na chmurze]: https://sendgrid.com/email-solutions
  [dostarczanie transakcji poczty e-mail]: https://sendgrid.com/transactional-email
 
