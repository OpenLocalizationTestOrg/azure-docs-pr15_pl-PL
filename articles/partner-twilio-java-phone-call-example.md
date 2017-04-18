<properties 
    pageTitle="Jak skonfigurować połączenie telefoniczne z Twilio (Java) | Microsoft Azure" 
    description="Dowiedz się, jak nawiązywanie połączenia telefonicznego ze strony sieci web przy użyciu Twilio w aplikacji języka Java Azure." 
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Jak skonfigurować połączenie telefoniczne przy użyciu Twilio w aplikacji języka Java Azure 

W poniższym przykładzie pokazano, jak Twilio umożliwia nawiązywanie połączenia ze strony sieci web hostowanej w Azure. Wynikowa aplikacji wyświetli monit dla wartości rozmowy telefonicznej, jak pokazano na poniższym obrazie zrzut.

![Formularz Azure połączenia przy użyciu Twilio i Java][twilio_java]

Konieczne będzie wykonaj poniższe czynności, aby użyć kodu w tym temacie:

1. Uzyskiwanie konta Twilio i uwierzytelniania token. Aby rozpocząć pracę z Twilio, oszacować ceny na [http://www.twilio.com/pricing][twilio_pricing]. Możesz zalogować w [https://www.twilio.com/try-twilio][try_twilio]. Uzyskać informacji na temat interfejsu API dostarczony przez Twilio, zobacz [http://www.twilio.com/api][twilio_api].
2. Uzyskanie Twilio SŁOIK. W [https://github.com/twilio/twilio-java][twilio_java_github], można pobrać GitHub źródeł i tworzyć własne SŁOIK lub pobieranie gotowych SŁOIK (z lub bez zależności).
Kod w tym temacie zostały zapisane przy użyciu wbudowanych SŁOIK TwilioJava 3.3.8 z zależności.
3. Dodawanie SŁOJU do ścieżki kompilacji Java.
4. Jeśli tworzenie tej aplikacji Java za pomocą Zaćmienie w pliku wdrażania aplikacji (też) za pomocą funkcji zestawu wdrożenia firmy Zaćmienie należy umieścić Twilio JAR. Jeśli nie używasz Zaćmienie do utworzenia tej aplikacji Java, upewnij się, Twilio JAR jest dostępny w obrębie tej samej roli Azure jako aplikacja języka Java i dodawana do ścieżki klasy aplikacji.
5. Upewnij się, do elementu keystore cacerts zawiera certyfikat urząd certyfikacji bezpiecznego firmy Equifax z MD5 odcisku palca 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (numer seryjny jest 35:DE:F4:CF i odcisku palca SHA1 jest D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Jest to urząd certyfikacji certyfikat dla [https://api.twilio.com] [ twilio_api_service] usługa, która jest wywoływana, gdy używasz interfejsy API Twilio. Informacje dotyczące dodawania ten certyfikat urzędu certyfikacji do magazynu cacert usługi JDK, zobacz [Dodawanie certyfikatu do magazynu certyfikatów urzędu certyfikacji Java][add_ca_cert].

Ponadto znajomości informacji podczas [tworzenia Witaj świecie aplikacji przy użyciu narzędzi Azure dla programu Eclipse][azure_java_eclipse_hello_world], lub z innych technik do obsługi aplikacji Java w Azure, jeśli nie używasz Zaćmienie jest zalecany.

## <a name="create-a-web-form-for-making-a-call"></a>Tworzenie formularza sieci web nawiązywania połączenia

Poniższy kod przedstawia sposób tworzenia formularza sieci web, aby pobrać dane użytkownika nawiązywania połączenia. W celu w tym przykładzie nowy projekt dynamiczne sieci web o nazwie **TwilioCloud**został utworzony, a **callform.jsp** zostało dodane jako plik JSP.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Utwórz kod, aby nawiązać połączenie
Następujący kod, który jest wywoływana, gdy użytkownik kończy formularza były wyświetlane przez callform.jsp, tworzy wiadomość połączenia i generuje połączenie. Na potrzeby tego przykładu plik JSP nosi nazwę **makecall.jsp** i została dodana do projektu **TwilioCloud** . (Używaj swojego konta Twilio i uwierzytelniania token zamiast wartości symbol zastępczy przypisane do **accountSID** i **parametr authToken** w kodzie poniżej).

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Oprócz nawiązywania połączenia, makecall.jsp wyświetla punkt końcowy Twilio, wersja API i stan połączenia. Przykład przedstawia się następujące zrzut ekranu:

![Odpowiedź Azure połączenia przy użyciu Twilio i Java][twilio_java_response]

## <a name="run-the-application"></a>Uruchamianie aplikacji
Poniżej przedstawiono czynności wysokiego poziomu na uruchomienie aplikacji; Szczegóły dla tych czynności można znaleźć na [Tworzenie Witaj świecie aplikacji za pomocą narzędzi Azure dla programu Eclipse][azure_java_eclipse_hello_world].

1. Eksportowanie do też TwilioCloud do folderu Azure **approot** . 
2. Modyfikowanie **startup.cmd** do plików z też TwilioCloud.
3. Kompilacji aplikacji dla emulatora obliczeń.
4. Uruchom z wdrożeniem w emulatorze obliczeń.
5. Otwórz przeglądarkę sieci, a następnie uruchom **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Wprowadź wartości w formularzu, kliknij **ten połączenia**, a następnie wyświetlić wyniki w makecall.jsp.

Po zakończeniu wdrożenia Azure, ponownej kompilacji do wdrożenia w chmurze, wdrażania Azure i http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp są uruchamiane w przeglądarce (wartość dla *your_hosted_name*podstaw).

## <a name="next-steps"></a>Następne kroki
Kod podano Tobie podstawowe funkcje za pomocą Twilio w języku Java Azure. Przed wdrożeniem Azure produkcji, można dodać więcej obsługi błędów i innych elementów. Na przykład:

* Zamiast używać formularza sieci web, można Azure magazyn obiektów blob lub bazy danych SQL do przechowywania numerów telefonów i połączeń tekstu. Aby dowiedzieć się, jak przy użyciu obiektów blob platformy Azure miejsca do magazynowania w języku Java, zobacz, [jak korzystać z usługi Magazyn obiektów Blob Java][howto_blob_storage_java]. Uzyskać informacji o korzystaniu z bazy danych SQL w języku Java, zobacz [Za pomocą bazy danych SQL w języku Java][howto_sql_azure_java].
* W obszarze Ustawienia konfiguracji z wdrożeniem, zamiast słabo kodowania wartości w makecall.jsp, można użyć **RoleEnvironment.getConfigurationSettings** Pobierz identyfikator konta Twilio i token uwierzytelniania. Aby uzyskać informacji na temat klasy **RoleEnvironment** , zobacz [Korzystanie biblioteki środowisko uruchomieniowe usługi Azure w JSP] [ azure_runtime_jsp] i dokumentację pakietu środowisko uruchomieniowe usługi Azure w [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Kod makecall.jsp przypisuje dostarczonego przez Twilio adresu URL, [http://twimlets.com/message][twimlet_message_url], zmiennej **adresu Url** . Ten adres URL zawiera odpowiedź Twilio Markup Language (TwiML), która zostanie wyświetlona informacja Twilio, jak przystąpić do połączenia. Na przykład może zawierać TwiML, który został zwrócony ** &lt;powiedz&gt; ** czasownika powstały w tekście wymowę odbiorcy. Zamiast używać dostarczonego przez Twilio adres URL, można utworzyć usługi na odpowiedź na żądanie w Twilio; Aby uzyskać więcej informacji, zobacz [jak Twilio Użyj głosowe i możliwości SMS Java][howto_twilio_voice_sms_java]. Więcej informacji na temat TwiML można znaleźć w [http://www.twilio.com/docs/api/twiml][twiml]oraz więcej informacji o ** &lt;powiedz&gt; ** i inne zlecenia Twilio można znaleźć w [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Przeczytaj wskazówki dotyczące zabezpieczeń Twilio w [https://www.twilio.com/docs/security][twilio_docs_security].

Aby uzyskać dodatkowe informacje o Twilio, zobacz [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Zobacz też
* [Jak używać Twilio głosowe i możliwości SMS Java][howto_twilio_voice_sms_java]
* [Dodawanie certyfikatu w magazynie certyfikatów urzędu certyfikacji Java][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
