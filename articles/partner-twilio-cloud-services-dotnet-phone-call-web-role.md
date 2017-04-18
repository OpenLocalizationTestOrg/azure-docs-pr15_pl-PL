<properties 
    pageTitle="Jak skonfigurować połączenie telefoniczne z Twilio (.NET) | Microsoft Azure" 
    description="Dowiedz się, jak nawiązywanie połączenia telefonicznego i wysyłanie wiadomości SMS za pomocą interfejsu API Twilio usługi Azure. Przykłady kodu napisanego w .NET." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Jak skonfigurować połączenie telefoniczne przy użyciu Twilio w roli web Azure

Ten przewodnik przedstawiono sposób użycia Twilio nawiązać połączenie ze strony sieci web hostowanej w Azure. Wynikowa aplikacji monituje użytkownika w przypadku wartości rozmowy telefonicznej, jak pokazano w poniższej zrzut ekranu.

![Formularz Azure połączenia przy użyciu Twilio i ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Wymagania wstępne

Konieczne będzie wykonaj poniższe czynności, aby użyć kodu w tym temacie:

1. Uzyskiwanie konta Twilio i uwierzytelniania token. Aby rozpocząć pracę z Twilio, Utwórz konto na [https://www.twilio.com/try-twilio][try_twilio]. Możesz oszacować ceny na [http://www.twilio.com/pricing][twilio_pricing]. Uzyskać informacji na temat interfejsu API dostarczony przez Twilio, zobacz [http://www.twilio.com/voice/api][twilio_api].
2. Dodawanie biblioteki Twilio .NET do ról w sieci web. Zobacz "Dodawanie bibliotek Twilio do projektu sieci web roli" w dalszej części tego tematu.

Należy się zapoznać z tworzenie roli podstawowe web Azure.

## <a name="howtocreateform"></a>Jak: Tworzenie formularza sieci web nawiązywania połączenia

<a id="use_nuget"></a>Aby dodać bibliotek Twilio do projektu ról w sieci web:

1.  Otwórz rozwiązania w programie Visual Studio.
2.  Kliknij prawym przyciskiem myszy **odwołania**.
3.  Kliknij pozycję **Zarządzaj NuGet pakietów**.
4.  Kliknij pozycję **Online**.
5.  W polu wyszukiwania online wpisz *twilio*.
6.  Kliknij przycisk **Zainstaluj** na opakowaniu Twilio.

Poniższy kod przedstawia sposób tworzenia formularza sieci web, aby pobrać dane użytkownika nawiązywania połączenia. W tym przykładzie jest tworzona rolę sieci web programu ASP.NET o nazwie **TwilioCloud** .

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Jak: tworzenie kod, aby nawiązać połączenie
Następujący kod, który jest wywoływana, gdy użytkownik zamyka formularz, tworzy wiadomość połączenia i generuje połączenie. W tym przykładzie kodu uruchamianego w program obsługi zdarzeń przycisk formularz. (Używaj swojego konta Twilio i uwierzytelniania token zamiast wartości symbol zastępczy przypisane do **accountSID** i **parametr authToken** w kodzie poniżej).

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

Połączenie zostanie nawiązane, a punkt końcowy Twilio, wersja API i stan połączenia są wyświetlane. Następujące zrzut ekranu przedstawia dane wyjściowe z uruchamiania próbki.

![Odpowiedź Azure połączenia przy użyciu Twilio i programu ASP.NET][twilio_dotnet_basic_form_output]

Więcej informacji na temat TwiML można znaleźć w [http://www.twilio.com/docs/api/twiml][twiml]. Więcej informacji na temat &lt;powiedz&gt; i inne zlecenia Twilio można znaleźć w [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Następne kroki
Kod podano Tobie podstawowe funkcje przy użyciu Twilio w roli sieci web programu ASP.NET Azure. Przed wdrożeniem Azure produkcji, można dodać więcej obsługi błędów i innych elementów. Na przykład:

* Zamiast używać formularza sieci web, można magazyn obiektów Blob platformy Azure lub wystąpienia bazy danych SQL Azure do przechowywania numerów telefonów i połączeń tekstu. Aby dowiedzieć się, jak przy użyciu obiektów blob platformy Azure, zobacz, [jak korzystać z usługi Magazyn obiektów Blob platformy Azure w .NET][howto_blob_storage_dotnet]. Aby uzyskać informacje o korzystaniu z bazy danych SQL, zobacz, [jak używać bazy danych SQL Azure w aplikacjach .NET][howto_sql_azure_dotnet].
* W obszarze Ustawienia konfiguracji z wdrożeniem, zamiast twardym kodowania wartości w formularzu, można użyć RoleEnvironment.getConfigurationSettings Pobierz identyfikator konta Twilio i token uwierzytelniania. Informacje klasy RoleEnvironment, zobacz [Namespace Microsoft.WindowsAzure.ServiceRuntime][azure_runtime_ref_dotnet].
* Przeczytaj wskazówki dotyczące zabezpieczeń Twilio w [https://www.twilio.com/docs/security][twilio_docs_security].
* Dowiedz się więcej o Twilio w [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Zobacz też
* [Jak używać Twilio głosowe i możliwości SMS z platformy Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
