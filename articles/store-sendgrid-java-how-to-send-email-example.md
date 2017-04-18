<properties 
    pageTitle="store-sendgrid-Java-How-to-send-email-example" 
    description="Jak wysłać wiadomości e-mail za pomocą SendGrid z języka Java w wdrożenia usługi Azure" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Jak wysłać wiadomości E-mail za pomocą SendGrid z języka Java w Azure wdrożenia

W poniższym przykładzie pokazano, jak SendGrid umożliwia wysyłanie wiadomości e-mail ze strony sieci web hostowanej w Azure. Wynikowa aplikacji wyświetli monit dla wartości poczty e-mail, jak pokazano na poniższym obrazie zrzut.

![Formularz poczty e-mail][emailform]

Otrzymane wiadomości e-mail będzie wyglądać podobnie do następujących zrzut ekranu.

![Wiadomości e-mail][emailsent]

Konieczne będzie wykonaj poniższe czynności, aby użyć kodu w tym temacie:

1. Na przykład uzyskać słoików javax.mail <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Dodawanie słoików do ścieżki kompilacji Java.
3. Jeśli korzystasz z Zaćmienie do utworzenia tej aplikacji Java, możesz umieścić bibliotek SendGrid w pliku wdrażania aplikacji (też) za pomocą funkcji zestawu wdrożenia Zaćmienie firmy. Jeśli nie używasz Zaćmienie do utworzenia tej aplikacji Java, upewnij się, bibliotek są zawarte w tej samej roli Azure jako aplikacja języka Java i dodawana do ścieżki klasy aplikacji.


Musisz także własne SendGrid użytkownika i hasło, aby można było wysyłać wiadomości e-mail. Aby rozpocząć pracę z SendGrid, zobacz [temat wysyłania wiadomości e-mail za pomocą SendGrid z języka Java](store-sendgrid-java-how-to-send-email.md).

Ponadto znajomości informacje [Tworzenie aplikacji Witaj świecie dla Azure w Zaćmienie](http://msdn.microsoft.com/library/windowsazure/hh690944)lub innych technik do obsługi aplikacji Java w Azure, jeśli nie używasz Zaćmienie, zaleca się.

## <a name="create-a-web-form-for-sending-email"></a>Tworzenie formularza sieci web do wysyłania wiadomości e-mail

Poniższy kod przedstawia sposób tworzenia formularza sieci web, aby pobrać dane użytkownika do wysyłania wiadomości e-mail. Na potrzeby tej zawartości plik JSP nosi nazwę **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>Utwórz kod, aby wysłać wiadomość e-mail

Następujący kod, który jest nazywany po zakończeniu formularz w emailform.jsp, tworzy wiadomości e-mail i wysyła go. Na potrzeby tej zawartości plik JSP nosi nazwę **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Oprócz podczas wysyłania wiadomości e-mail, emailform.jsp przewiduje wyniku użytkownika; przykład przedstawia się następujące zrzut ekranu:

![Wysyłanie poczty wynik][emailresult]

## <a name="next-steps"></a>Następne kroki

Wdrażanie aplikacji emulatora obliczeń i w przeglądarce uruchamianie emailform.jsp, wprowadź wartości w formularzu, kliknij przycisk **Wyślij tę wiadomość e-mail**i wyświetlanie wyników w sendemail.jsp.

Kod dostarczono pokazano, jak za pomocą SendGrid w języku Java Azure. Przed wdrożeniem Azure produkcji, można dodać więcej obsługi błędów i innych elementów. Na przykład: 

* Azure magazyn obiektów blob lub bazy danych SQL można użyć do przechowywania adresów e-mail i wiadomości e-mail, a nie przy użyciu formularza sieci web. Aby dowiedzieć się, jak przy użyciu obiektów blob platformy Azure miejsca do magazynowania w języku Java zobacz [jak korzystać z usługi Magazyn obiektów Blob Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Aby dowiedzieć się, jak za pomocą bazy danych SQL w języku Java zobacz [Za pomocą bazy danych SQL w języku Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Można użyć `RoleEnvironment.getConfigurationSettings` pobrać SendGrid nazwy użytkownika i hasła z ustawienia konfiguracji z wdrożeniem, zamiast formularza sieci web, aby pobrać te wartości. Aby uzyskać informacje o `RoleEnvironment` klasy, zobacz [Korzystanie z biblioteki środowisko uruchomieniowe usługi Azure w JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) i dokumentację pakietu środowisko uruchomieniowe usługi Azure w <http://dl.windowsazure.com/javadoc>.
* Aby uzyskać więcej informacji o korzystaniu z SendGrid w języku Java zobacz [jak wysyłać wiadomości e-mail za pomocą SendGrid z języka Java](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
